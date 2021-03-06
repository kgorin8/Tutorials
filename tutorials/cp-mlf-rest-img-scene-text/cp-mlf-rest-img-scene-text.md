---
title: Use the Scene Text Recognition API from a REST Client
description: Discover how to call the Scene Text Recognition API from a REST Client like Postman
auto_validation: true
time: 10
tags: [tutorial>beginner, topic>cloud, topic>machine-learning, products>sap-cloud-platform, products>sap-cloud-platform-for-the-cloud-foundry-environment]
primary_tag: products>sap-leonardo-machine-learning-foundation
---

## Prerequisites
 - [Create a Machine Learning Foundation service instance on the Cloud Foundry environment](https://developers.sap.com/tutorials/cp-mlf-create-instance.html)
 - [Install Postman as a REST client](https://developers.sap.com/tutorials/api-tools-postman-install.html)
 - [Get your OAuth Access Token using a REST Client](https://developers.sap.com/tutorials/cp-mlf-rest-generate-oauth-token.html)

## Details
### You will learn
  - Call an API from a REST client like Postman
  - The basics about Machine Learning Foundation Service for Scene Text Recognition

[ACCORDION-BEGIN [Step](The Scene Text Recognition Service)]

The Scene Text Recognition service localizes and reads text from natural images and scenes.

> ***Differences with the OCR service***
In comparison to the Optical Character Recognition service, the Scene Text Recognition service offers
>
- Works with real-life color images
- Ability to work with font-less text
- Extract word-art / `picturized` text
- Works in different orientations of texts
- Text occurring in natural images like low-contrast, emboss/engrave

>When the formats from which the text has to be read are documents or print media scans, the OCR service should be used whereas in case of natural images (e.g. reading the counter of a utility meter or the number-plate of an automobile), the Scene Text Recognition service should be used.

This is the list of accepted file extensions:

|Name                  | Description
|----------------------|--------------------
| **Image file**       | `jpg`, `jpe`, `jpeg`, `png`

The images should be RGB.

The input file is provided using form data (as an element named ***files*** in the form data).

The service will return a JSON response that includes the detected texts within the scene along with the bounding box coordinates.

For more details, you can check the following link:

 - [Inference Service for Scene Text Recognition on the SAP API Business Hub](https://api.sap.com/api/scene_text_recognition_api/resource).

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step](Call the API)]

Open a new tab in ***Postman***.

Make sure that the ***`my-ml-foundation`*** environment is selected.

On the **Authorization** tab, select **Bearer Token**, then enter **`{{OAuthToken}}`** as value.

![Postman](01.png)

> ### **Note:**: the **`OAuthToken`** environment variable can be retrieved following the [Get your OAuth Access Token using a REST Client](https://developers.sap.com/tutorials/cp-mlf-rest-generate-oauth-token.html) tutorial.

Fill in the following additional information:

Field Name               | Value
:----------------------- | :--------------
<nobr>HTTP Method</nobr> | POST
<nobr>URL<nobr>          | the value for **`SCENE_TEXT_RECOGNITION_URL`** in your service key

> **Note** As a reminder, the URL depends on you Cloud Platform landscape region but for the trial landscape only ***Europe (Frankfurt)*** provide access to the Machine Learning Foundation services.

On the **Body** tab, keep **`form-data`** selected. Add a new key named **files** and switch it to **File** instead of **Text** (default).

Select your image file.

![Postman](02.png)

If you are missing some inspiration, you can use the following pictures from [SAP TechEd Barcelona 2018](https://sapteched2018.event-hosting.com/Gallery-336659.html):

 - <a href="https://sapteched2018.event-hosting.com/srv/ds/custom/download?size=2048&images=551006" target="blank" download="SAP_TechEd_BCN2018_01020.jpg">Image `SAP_TechEd_BCN2018_01020`</a>
 - <a href="https://sapteched2018.event-hosting.com/srv/ds/custom/download?size=2048&images=551064" target="blank" download="SAP_TechEd_BCN2018_01256.jpg">Image `SAP_TechEd_BCN2018_01256`</a>

The first image (`SAP_TechEd_BCN2018_01020.jpg`) contains the following text :

  - SAP TechEd Registration Check-In

The second image (`SAP_TechEd_BCN2018_01256.jpg`) contains the following text :

  - Pleas have your Photo `I.D.` ready

Click on **Send**.

You should receive a response that includes a series of entries:

![Postman](03.png)

```json
{
    "imageName": "SAP_TechEd_BCN2018_01020.jpg",
    "results": [
        {
            "bboxAccuracy": 0.9788690209388733,
            "boundingBox": {
                "x1": 1139,
                "x2": 1477,
                "x3": 1482,
                "x4": 1144,
                "y1": 291,
                "y2": 276,
                "y3": 402,
                "y4": 417
            },
            "text": "Check-ll"
        },
        {
            "bboxAccuracy": 0.981939971446991,
            "boundingBox": {
                "x1": 799,
                "x2": 1098,
                "x3": 1104,
                "x4": 804,
                "y1": 381,
                "y2": 365,
                "y3": 477,
                "y4": 493
            },
            "text": "Registration"
        },
        {
            "bboxAccuracy": 0.977262020111084,
            "boundingBox": {
                "x1": 530,
                "x2": 632,
                "x3": 640,
                "x4": 538,
                "y1": 472,
                "y2": 458,
                "y3": 516,
                "y4": 530
            },
            "text": "TECHED"
        }
    ]
}
```

Each entry in the response represents a box that identify a bounding box with an associated text.

![example](04.png)

Here is a simple HTML code you can use to visualize other results:

```HTML
<html>
<body>
<div id="div">
<script>
var fontSize = 14;
var fontSpacer = parseInt(fontSize*1.2)
var response = {
    "id": "37ec14b4-0fd5-45ad-4c36-1c7775a35a1a",
    "predictions": [
        {
            "imageName": "https://sapteched2018.event-hosting.com/srv/ds/custom/download?size=2048&images=551006",
            "results": [
                {
                    "bboxAccuracy": 0.9788690209388733,
                    "boundingBox": {
                        "x1": 1139,
                        "x2": 1477,
                        "x3": 1482,
                        "x4": 1144,
                        "y1": 291,
                        "y2": 276,
                        "y3": 402,
                        "y4": 417
                    },
                    "text": "Check-ll"
                },
                {
                    "bboxAccuracy": 0.981939971446991,
                    "boundingBox": {
                        "x1": 799,
                        "x2": 1098,
                        "x3": 1104,
                        "x4": 804,
                        "y1": 381,
                        "y2": 365,
                        "y3": 477,
                        "y4": 493
                    },
                    "text": "Registration"
                },
                {
                    "bboxAccuracy": 0.977262020111084,
                    "boundingBox": {
                        "x1": 530,
                        "x2": 632,
                        "x3": 640,
                        "x4": 538,
                        "y1": 472,
                        "y2": 458,
                        "y3": 516,
                        "y4": 530
                    },
                    "text": "TECHED"
                }
            ]
        }
    ],
    "processedTime": "2018-11-14T15:49:03.027874+00:00",
    "status": "DONE"
};

function createCanvas(oImg, items) {
    var oCanvas = document.createElement("CANVAS");
    var ctx = oCanvas.getContext("2d");

    function resizeCanvas() {
        // set the image width to the window width
        imgRatioW = oImg.naturalWidth / window.innerWidth;
        oImg.width  = window.innerWidth;
        oImg.height = oImg.naturalHeight / imgRatioW ;
        // adjust the canvas dimension
        oCanvas.width  = oImg.width;
        oCanvas.height = oImg.height;
        // get the width & height for the image / canvas
        var itemRatioW = oCanvas.width  / oImg.naturalWidth;
        var itemRatioH = oCanvas.height / oImg.naturalHeight;
        // draw the canvas
        ctx.drawImage(oImg, 0, 0, oImg.width, oImg.height);
        ctx.lineWidth="3";
        ctx.strokeStyle="red";
        ctx.fillStyle = "white";
        ctx.font = fontSize + "px Arial";
        // display the items
        for (var i = 0; i < items.length; i++) {
            var text = "#" + i  + ": " + items[i].text + " " + ( items[i].bboxAccuracy * 100 ).toFixed(2) + "%";
            // draw the box
            ctx.moveTo(items[i].boundingBox.x1 * itemRatioW, items[i].boundingBox.y1 * itemRatioH);
            ctx.lineTo(items[i].boundingBox.x2 * itemRatioW, items[i].boundingBox.y2 * itemRatioH);
            ctx.lineTo(items[i].boundingBox.x3 * itemRatioW, items[i].boundingBox.y3 * itemRatioH);
            ctx.lineTo(items[i].boundingBox.x4 * itemRatioW, items[i].boundingBox.y4 * itemRatioH);
            ctx.lineTo(items[i].boundingBox.x1 * itemRatioW, items[i].boundingBox.y1 * itemRatioH);
            ctx.stroke();
            ctx.save();
            // write the text with the box angle
            ctx.translate(items[i].boundingBox.x4 * itemRatioW , items[i].boundingBox.y4 * itemRatioH);
            // do some trigonometry to calculate the angle in radian
            var ab = Math.abs(items[i].boundingBox.x2 - items[i].boundingBox.x1);
            var ac = Math.abs(items[i].boundingBox.y2 - items[i].boundingBox.y1);
            var bc = Math.sqrt( Math.pow(ab, 2) + Math.pow(ac, 2));
            var clockwise = (items[i].boundingBox.y2 - items[i].boundingBox.y1 > 0 ? 1 : -1);
            var radian = Math.acos(ab / bc);
            ctx.rotate( clockwise * radian );
            // draw the background box
            ctx.fillStyle = 'red';
            ctx.fillRect(-1, -1, ctx.measureText(text).width + 1, fontSpacer + 2);

            // write the text in the box
            ctx.fillStyle = "white";
            ctx.fillText(text, 0, fontSize);
            ctx.restore();
        }
    }
    // call the resize function
    resizeCanvas();
    // add the event listener
    window.addEventListener('resize', resizeCanvas, false);
    return oCanvas;
}

window.onload = function() {
	for (var idx = 0; idx < response.predictions.length; idx++) {
        var items = response.predictions[idx].results;
        var name = response.predictions[idx].imageName;

        var oImg = document.createElement("IMG");
        //document.getElementById("div").appendChild(oImg);
        oImg.onload = function(){
            document.getElementById("div").appendChild(createCanvas(this, items));
        }
        oImg.src = name;
	}
};
</script>
</div>
</body>
</html>
```

> ###**Note:**
> In order to use this sample HTML code, you will need to save the images in the same location as the HTML code using the same name used when submitting the service call (in the current code: `SAP_TechEd_BCN2018_01020.jpg`).

You will notice that the coordinates are not rectangles, but are 4 points x and y.

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step](Validate your results)]

Provide an answer to the question below then click on **Validate**.

[VALIDATE_1]
[ACCORDION-END]
