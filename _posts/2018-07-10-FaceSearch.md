---
layout: post
hero-bg-color: "#000"
title:  "FaceSearch"
date: 2018-07-10
category: projects
github: FaceSearch
thumb: "FaceSearch.png"

subtitle: "Know who's that unknown person in the image!"
worktype: "OpenCV"
summary: "FaceSearch: Searches for faces in a given image using OpenCV and the Google Reverse Image Search engine."
#progress: 100
---

## Abstract
FaceSearch: Searches for faces in a given image using OpenCV and the Google Reverse Image Search engine.

---

## Motivation
This occurred to me while scrolling through my news feed that there were a lot of unknown people in many of the photos. Some of the photos also included a few people I already knew and followed posing with those 'strangers'. Happens quite often with most of us. Most of the times, we simply ignore or try to google (and retreat). We don't really have any idea what to do in such a scenario! After 10–15 minutes of Google searching and/or Google _dorking_, we sometimes even achieve our goal; but nonetheless, not in a very straightforward way. I had learned OpenCV recently and having mostly gone through a lot of MOOCs in the summer (doing nothing but learning stuff), I thought why not give it a try! 

## The way to go 
The Google reverse image search comes in handy when you can't tell Google what you want, in words. The case is similar here. We know the face, not the name. We can use it. However, just running an image search for the entire image wouldn't be much fruitful, as it would most likely return you the same source from where you got the image!

We can, however, fine-tune the task and force it to search for a particular face in the image by using OpenCV and its Cascade Classifier. Searching for only the face turned out to be giving better results. We also need to upload the cropped image automatically to Google and open a new browser window with the results. A little bit of research showed it to be possible without any API, since Google has a static link to which one can simply send a POST request (using requests in python) to upload an image. The returned response contains a header named 'Location' which is the URL of search results found by Google. We can then use Python's built-in `webbrowser` module to open the URL in the user's browser. That's all we'll need. 

## Implementation
The script is a command line application. Once installed, you just have to do `facesearch path/to/image` to run the script on an image. OpenCV is an open-source computer vision library originally written in C++. It is widely used for computer vision related tasks. For the curious, computer vision is the field of AI that deals with making the computers 'see', i.e., work with images and derive useful data from it. OpenCV was used to detect the faces in the given image, which were then cropped out using `Numpy`, a python library for performing advanced mathematical stuff in python. Mostly used for working with matrices. The cropped faces are then padded appropriately and shown to user in a window, using OpenCV. OpenCV provides a dedicated function (`cv2.setMouseCallback`) to track the Mouse events on a window. It returns us the event name and the (x, y) coordinates of the event. Using the x, y coordinates of the click event, we can figure out which face user has clicked on. This allows for convenient point-and-click operation. For the convenience of dragging any image right off the internet and onto the terminal to get it searched, the script checks if the path provided to it is an internet URL and if it is, uses `urllib` to `GET` the image for further operations.

At last, to search for the selected face, the cropped out face is saved temporarily in the current directory and then uploaded to Google's server using the static link. Once the image is uploaded and the response is received back, the location header is read and passed to the `webbrowser` module, which then opens a new browser window or a new tab in an existing session with the search results. The script then cleans up the generated image and exits.

---

## Installation

1. Directly download the latest release from the [releases](https://github.com/IAmSuyogJadhav/FaceSearch/releases) page.

  **OR**
2. First clone this repo. Now, to install the dependencies and create the alias for FaceSearch, run the `install.sh`.
  ``` bash
  bash install.sh
  ```

## Usage
Once it finishes, you can now use the following command on the terminal to detect and search for the faces in any image.
``` bash
facesearch path/to/Image
```
**Also**, note that the `path/to/Image` can be an internet URL as well! (prefixed with http: or https:)
So, you can just drag an image off the internet over the terminal to get its URL pasted over there and search for faces in it using FaceSearch. Really convenient.

## Examples
Test image:

![alt text](/images/facesearch/test.jpg "Test image")

On command line:
```
anon@anon-pc:~/FaceSearch$ facesearch example/test.jpg
[ INFO:0] Initialize OpenCL runtime...
Uploading image..
Thanks for using this tool! Please report any issues to github.
https://github.com/IAmSuyogJadhav/FaceSearch/issues
anon@anon-pc:~/FaceSearch$ Created new window in existing browser session.
█
```
Output Window:

![alt text](/images/facesearch/test0.png "The output window")

In the browser:

![alt text](/images/facesearch/test1.png "Browser output")

Any feedback, bug reports and issues are welcome [here](https://github.com/IAmSuyogJadhav/FaceSearch/issues/new)!

## Updates
- 18-10-18: Created and released the first release of FaceSearch (v1.0). Directly downloadable from [here](https://github.com/IAmSuyogJadhav/FaceSearch/releases).
- 12-10-18: Thanks to Kaj Jansen, fixed an easy-to-miss bug. The script previously used the default system python; but, this caused an issue because of the `list.copy()` method not being defined in Python 2. Changed the script to explicitly use Python 3 now. Also, added a function in `install.sh` to remove any redundant aliases defined by the same name `facesearch`, to prevent problems occured because of running `install.sh` script multiple times.
- 10-08-18: Added support for closing the output window by GUI [x] button.
- 07-08-18: The project report is now ready! You can read it [here](/files/facesearch/Project_Report.pdf).
- 05-08-18: A blog post detailing the implementation and working of FaceSearch is live now. Read it [here](https://mlendeavours.wordpress.com/2018/08/05/facesearch/).


Image Source: [Rediff](http://im.rediff.com/getahead/2018/feb/26tanmay1.jpg)
