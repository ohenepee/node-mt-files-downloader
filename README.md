# NodeJS Download Manager

This is an up-to-date module that lets you:

- Manage multiple downloads
- Get stats (speed, eta, completed, etc)
- Auto-retry (continue) a download in case of error (ie. network error)
- Manually resume a download from partial file
- Stop and resume downloads
- Get notified by events when a download start, fail, retry, stopped, destroyed or complete

## Usage

Require the module:

	var Downloader = require('mt-files-downloader');

Create a new Downloader instance:

	var downloader = new Downloader();

Create a new download:

	var dl = downloader.download('FILE_URL', 'FILE_SAVE_PATH');

Start the download:

	dl.start();

## Events

You can then listen to those events :

- `dl.on('start', function(dl) { ... });`
- `dl.on('error', function(dl) { ... });`
- `dl.on('end', function(dl) { ... });`
- `dl.on('stopped', function(dl) { ... });`
- `dl.on('destroyed', function(dl) { ... });`
- `dl.on('retry', function(dl) { ... });`

## Downloader object

### Methods

- download(URL, FILE_SAVE_PATH, [options])
    - URL : URL of the file to download
    - FILE_SAVE_PATH : where to save the file (including filename !)
    - options : optional, passed directly to Download object
- resumeDownload(filePath) : create a new download by resuming from an existing file
- getDownloads() : get the list of downloads in manager
- getDownloadByUrl(url) : get a specified download by URL
- getDownloadByFilePath(filePath) : get a specified download by file path
- removeDownloadByFilePath(filePath) : remove a specified download by file path. It does not destroy it, just remove from download manager ! Call download.destroy() before if you want to completely remove it.

### Formatters methods

The Downloader object exposes some formatters for the stats as static methods :

- Downloader.Formatters.speed(speed)
- Downloader.Formatters.elapsedTime(seconds)
- Downloader.Formatters.remainingTime(seconds)

## Download object

### Properties

- status : 
    - -3 = destroyed
    - -2 = stopped
    - -1 = error
    - 0 = not started
    - 1 = started (downloading)
    - 2 = error, retrying
    - 3 = finished
- url
- filePath
- options
- meta

### Methods

- setUrl(url) : set the download URL
- setFilePath(path) : set the download file save path
- setOptions(options) : set the download options
    - threadsCount: Default: 2, Set the total number of download threads
    - method: Default: GET, HTTP method
    - port: Default: 80, HTTP port
    - timeout: Default: 5000, If no data is received, the download times out (milliseconds)
    - range: Default: 0-100, Control the part of file that needs to be downloaded.
- setRetryOptions(options) : set the retry options
    - maxRetries: Default 5, max number of retries before considering the download as failed
    - retryInterval: Default 2000, interval (milliseconds) between each retry
- setMeta(meta) : set download metadata
- setStatus(status) : set download status
- setError(error) : set error message for download
- getStats() : compute and get stats for the download
- start() : start download
- resume() : resume download
- stop() : stop the download, keep the files
- destroy() : stop the download, remove files
