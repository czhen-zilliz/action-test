const axios = require("axios");
const fs = require("fs");
const fm = require("front-matter");

const args = process.argv.slice(2);
// const MEDIUM_POST_API = "https://api.medium.com/v1/users/{{authorId}}/posts";
const MEDIUM_POST_API =
process.env.MEDIUM_POST_API ||
"http://localhost:3001/v1/users/{{authorId}}/posts";
const MEDIUM_POST_TOKEN = process.env.MEDIUM_POST_TOKEN || "token test";

console.log("---- args ----", args);

const post2Medium = async (body) => {
// const res = await axios.post(MEDIUM_POST_API, body, {
// headers: {
// Authorization: `Bearer ${MEDIUM_POST_TOKEN}`,
// ContentType: "application/json",
// Accept: "application/json",
// AcceptCharset: "utf-8",
// },
// });
console.log("---- post medium ----", body);
return body;
};

const readMdFiles = async (pathList = []) => {
const result = [];
const failure = [];
for (const filePath of pathList) {
if (filePath.startsWith("blog/")) {
try {
const fileContent = fs.readFileSync(filePath, "utf-8");
const fileData = fm(fileContent);
// console.log("fileData", fileData);
const {
attributes: {
title,
contentFormat = "markdown",
tags,
canonicalUrl,
publishStatus,
license,
notifyFollowers,
} = {},
} = fileData;
if (!title) {
throw Error("title should not be empty.");
}
const requestBody = { title, contentFormat, content: fileContent };
tags && (requestBody.tags = tags);
canonicalUrl && (requestBody.canonicalUrl = canonicalUrl);
publishStatus && (requestBody.publishStatus = publishStatus);
license && (requestBody.license = license);
notifyFollowers && (requestBody.notifyFollowers = notifyFollowers);

        const res = await post2Medium(requestBody);
        result.push({ ...res, filePath });
      } catch (error) {
        console.error(`---- error [${filePath}] ----`, error);
        failure.push(filePath);
      }
    }

}

console.log(`==== Results Details ====`);
console.log(`Success: `, result);
console.log(`Failed: `, failure);

console.log(`==== Results Summary ====`);
console.log(
`Total: ${pathList.length} success: ${result.length} failed: ${failure.length} `
);
};

readMdFiles(args);
