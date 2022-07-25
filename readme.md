# GD Share
## demo
[https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2](https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2)

## Introduction
This is a project inspired by goindex, suitable for deployment on cloudflare workers, and has the following features compared to the original version

-Full disk search (including personal disk and all authorized team disks, you can click the link in the search result to jump to the corresponding google drive official website)
- Page browsing (the number of files per page can be customized, and each page can be sorted by file name and size)
- More beautiful UI (thanks to ant design)
- Anti-crawler, for all directories and files, only the administrator has the permission to read and download (the original goindex can set the read password for the directory by preventing .password in the directory, but it cannot limit the download of a single file)
- A direct download link can be generated, which is convenient for third-party download tools to download. For streaming media files, you can directly open and play them with players such as potplayer (the validity period can be customized)
- A sharing link with an extraction code can be generated to facilitate sharing for others to browse and download (also supports custom validity period)

## changelog
### 2022-03-10
-Update the react-router-cache-route dependency to solve the phenomenon of stuck when the page is not fully loaded by moving forward/backward

-Move the static files in the template from jsdelivr to unpkg to speed up domestic access...

### 2020-08-12
- Display the team disk list (up to 100) on the homepage, click the corresponding name to enter
- Added the function of obtaining direct links in batches (you can choose by yourself, or you can get the straight links of all direct sub-files in the current directory with one click)
- Add evoke external player function (support IINA/PotPlayer/VLC/nPlayer/MXPlayer)
- Added a search function in a specified range (you can search on the directory list page. Due to the limitation of Google API, you can only search the direct sub-files of the current directory, and the contents of recursive sub-directories cannot be searched. If the current directory is the root directory of the team disk, then Support whole disk search)
- "Show Path" button now correctly identifies team drive names (previously returned "Drive")
- Added failure retry mechanism to backend search interface

## tips
- This tool can also be used as goindex, just add the directory ID to `https://your.website.com/ls/`.
For example, to browse the content of the team disk, you can directly visit `https://your.website.com/ls/your team disk ID`
Browsing the root directory of the personal disk can directly access `https://your.website.com/ls/root`

## Building Method
Open[template.js](./template.js)，Modify variables as prompted：
```javascript
const CONFIG = {
    PASSKEY: "this is your passkey", // Administrator webpage login key, please modify it yourself, try to be as complicated as possible
    HASHKEY: "this is your hash key", // The number of single-page objects to read the list, the official limit is up to 1000
    RETRY_LIMIT: 5, // Sometimes an error will be reported when calling the google drive api to read the directory, here is the maximum number of retries allowed
    PAGESIZE: 100, // 
    AUTH: {
        client_id: "insert_your_client_id", // These three items are your google account personal authorization information, the same as goindex
        client_secret: "insert_your_client_secret", // Ditto required
        refresh_token: "insert_your_refresh_token", // Ditto required 
        expires: 0,
        access_token: "" // 
    }
}
```
After the variables are set, copy template.js as a whole to the cloudflare worker (for specific steps, please refer to https://www.jiyiblog.com/archives/031279.html) and complete.
