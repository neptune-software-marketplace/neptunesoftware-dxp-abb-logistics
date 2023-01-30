sap.ui.getCore().attachInit(function(data) {
    setTimeout(function() {
        showMap();
        oPopover.openBy(btnDocDialog);
        if (sap.n) {
            sap.n.Shell.attachBeforeDisplay(function() {
                setTimeout(function() {
                    if (oApp.getCurrentPage().getId().includes("oPageSESDetails") && oIconTabBar.getSelectedKey() === "tabMap") {
                        oIconTabBar.fireSelect({
                            'key': 'tabMap'
                        });
                    } else if (oApp.getCurrentPage().getId().includes("oPageStart")) {
                        showMap();
                        map.setView(new L.LatLng(mapcenterlat, mapcenterlong), mapzoom);
                    } else if (oApp.getCurrentPage().getId().includes("oPageTimer")) {
                        var tmrData = JSON.parse(JSON.stringify(modeloPageSESDetails.oData));
                        initTimer(tmrData);
                    }
                }, 500);
            });
        }
    }, 1000);
});



document.addEventListener("deviceready", function() {
    pictureSource = navigator.camera.PictureSourceType;
    destinationType = navigator.camera.DestinationType;
    document.addEventListener('resume', function() {
        if (oApp.getCurrentPage().getId().includes("oPageSESDetails") && oIconTabBar.getSelectedKey() === "tabMap") {
            oIconTabBar.fireSelect({
                'key': 'tabMap'
            });
        } else if (oApp.getCurrentPage().getId().includes("oPageStart")) {
            showMap();
            map.setView(new L.LatLng(mapcenterlat, mapcenterlong), mapzoom);
        } else if (oApp.getCurrentPage().getId().includes("oPageTimer")) {
            var tmrData = JSON.parse(JSON.stringify(modeloPageSESDetails.oData));
            lblHH.setText("00");
            lblMM.setText("00");
            lblSS.setText("00");
            initTimer(tmrData);
        }
    }, false);
}, false);


// Called when a photo is successfully retrieved
function onPhotoDataSuccess(imageData) {
    var imgData = "data:image/jpeg;base64," + imageData;
    //imgData = iconDownload
    var nd = new Date();
    var ts = nd.getTime();
    var order = modeloPageSESDetails.oData.order;
    var doc = {
        'order': order,
        'ts': ts,
        'name': 'DOC-' + ts,
        'img': imgData,
        'timetaken': getTS(nd)
    };
    ModelData.Add(mdlDocs, doc);
    setCachemdlDocs();

    if (modelmdlDocs.oData.length) {
        var alldocs = JSON.parse(JSON.stringify(modelmdlDocs.oData));
        var doclist = alldocs.filter(doclist => doclist.order === order);
        modeloListDocs.setData(doclist);
        modeloListDocs.refresh(true);
        tabDocs.setText("Documents(" + doclist.length + ")");
    } else {
        tabDocs.setText("Documents(0)");
    }
}

// Called when a photo is successfully retrieved

function capturePhoto() {
    //  Take picture using device camera and retrieve image as base64-encoded string
    navigator.camera.getPicture(onPhotoDataSuccess, onFail, {
        quality: 50,
        correctOrientation: true,
        destinationType: destinationType.DATA_URL
    });
}

//********************

sap.ui.getCore().attachInit(function (data) {
    oHTMLObjectCameraUpload.setContent('<input type="file" accept="image/*" id="file-input"  style="display:none">');
});

function handleFileSelect(f) {
    console.log("Handling uploaded file")
    console.log(f)
    var reader = new FileReader();

    reader.onload = (function (theFile) {
        return function (e) {
            var binaryData = e.target.result;
            // Converting Binary Data to base 64
            var base64String = window.btoa(binaryData);

            var fullBase64picture = "data:image/png;base64," + base64String;

            var imageToStore = {
                "imageID": Date.now(),
                "base64data": fullBase64picture
            };

            var currentImages = modeloModelArrayImageStorage.getData();
            currentImages.push(imageToStore);
            modeloModelArrayImageStorage.setData(currentImages);
            console.log(modeloModelArrayImageStorage.getData());

            //modeloCarousel.setData(currentImages);

            // Set the currentImages to the list
            modeloListDocs.setData(currentImages)
            // Set up a customListItem with an Image component

            // if (currentImages.length > 0) {
            //     oButtonDeletePicture.setVisible(true);
            // } else {
            //     oButtonDeletePicture.setVisible(false);
            // }

            //console.log(modeloModelArrayImageStorage.getData());

        };
    })(f);

    reader.readAsBinaryString(f);

     setTimeout(function () {

         //oCarousel.next();

     }, 500);

}


// Called if something bad happens.
function onFail(message) {
    sap.m.MessageToast.show('Failed because: ' + message);
}