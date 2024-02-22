# GSheet_to_Git
GoogleSheet_to_Github(through apps script)

function main_function() {
  var githubToken = ""; // 替换为你的GitHub令牌
  var repo = "GSheet_to_Git"; // 你的GitHub仓库名
  var owner = "QifanGu"; // 你的GitHub用户名
  var path = "data.csv"; // 文件路径
  var sheetData = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet().getDataRange().getValues();
  var content = Utilities.base64Encode(JSON.stringify(sheetData)); // 将数据编码为Base64
  
  var apiUrl = "https://api.github.com/repos/" + owner + "/" + repo + "/contents/" + path;

  var payload = JSON.stringify({
    "message": "update file", // 提交信息
    "content": content // 编码后的内容
    // "sha": "file_sha" // 如果更新文件，需要文件的SHA
  });

  var headers = {
    "Authorization": "token " + githubToken,
    "Content-Type": "application/json"
  };

  var options = {
    "method": "put",
    "headers": headers,
    "payload": payload,
    "muteHttpExceptions": true
  };

  var response = UrlFetchApp.fetch(apiUrl, options);
  Logger.log(response.getContentText());
}
