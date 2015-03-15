# Introduction #
In the version 0.5.0 the **GWT Cropper** got a new class called _GWTCropperPreview.java_, that allows to show in real time only a selected area in separate panel (i.e. "preview"). It is pretty easy to use it. Here are two steps to set it up.

## Demo ##
Take a look at this widget [in action](http://wiki.gwt-cropper.googlecode.com/hg/demo/Application2.html)

## 1. Create Preview widget instance ##

```java

GWTCropperPreview preview = new GWTCropperPreview(Dimension.WIDTH, 100);
```

During initializing you must specify two arguments: a side of the widget that you wish to leave unchanged and its lenght in pixels. In case if you specified the free-shaped selection, the preview will have one side with constant length (width 100px in the example above) and adjust another one to the current shape of a selection.

Let's take an example to make it more understandable. Let's say you have a cropper with free-shaped selection and next to it you placed a preview widget. It is wise to make a constant width for this widget to save the layout of your page. When an user changes the selection, the preview widget will automatically adjust its height to save the correct proportion

<img src='http://wiki.gwt-cropper.googlecode.com/hg/apidocs/com/google/code/gwt/crop/client/doc-files/preview-fixed-width.jpg' />

_Pic.1 If you specify the WIDTH to preview widget, then it will adjust its height according the selection proportion_

On the contrary, if you placed a preview in a vertical panel it is wise to make a constant height to save the page layout. The widget will work in the same way, but adjusting its width:

<img src='http://wiki.gwt-cropper.googlecode.com/hg/apidocs/com/google/code/gwt/crop/client/doc-files/preview-fixed-height.jpg' />

_Pic.2 If you specify the HEIGHT to preview widget, then it will adjust its width according the selection proportion_

**Note**, if you specified aspect ratio 1, that means, that the widget has a sqare selection, then you can use any value for the first argument, because width is always equals height in a square. In this case these two lines will have the same effect:

```
        // in case of square selection these lines will have the same effect
        GWTCropperPreview previewW = new GWTCropperPreview(Dimension.WIDTH, 100);
        GWTCropperPreview previewH = new GWTCropperPreview(Dimension.HEIGHT, 100);
```

## 2. Register the preview widget in the cropper instance ##
Next step, just register this instance using the method [registerPreviewWidget(GWTCropperPreview previewWidget)](http://wiki.gwt-cropper.googlecode.com/hg/apidocs/com/google/code/gwt/crop/client/GWTCropper.html#registerPreviewWidget%28com.google.code.gwt.crop.client.GWTCropperPreview%29). The full example may look like this:

```
        GWTCropper cropper = new GWTCropper("image.jpg");
        GWTCropperPreview cropperPreview = new GWTCropperPreview(Dimension.WIDTH, 100);
        cropper.registerPreviewWidget(cropperPreview);
```

or the same example with a panel (more realistic):

```
        HorizontalPanel hp = new HorizontalPanel();
        hp.setSpacing(20);

        GWTCropper cropper = new GWTCropper("image.jpg");
        hp.add(cropper);

        GWTCropperPreview cropperPreview = new GWTCropperPreview(Dimension.WIDTH, 100);
        cropper.registerPreviewWidget(cropperPreview);
        hp.add(cropperPreview);
```

## Preview widget in UiBuilder ##
It is possible to declare these widgets in your _.ui.xml_ file. Here is a real-life example:

```
<ui:UiBinder xmlns:ui='urn:ui:com.google.gwt.uibinder'
    xmlns:g='urn:import:com.google.gwt.user.client.ui'
    xmlns:my="urn:import:com.google.code.gwt.crop.client">

  <g:HorizontalPanel spacing="20">

   <g:cell>
     <my:GWTCropper imageURL="image.jpg" ui:field="cropper"/>
   </g:cell>

   <g:cell>
     <g:HTMLPanel>
       <h1>Preview</h1>
       <my:GWTCropperPreview dimension="WIDTH" value="100" ui:field="cropperPreview" />
     </g:HTMLPanel>
    </g:cell>

  </g:HorizontalPanel>
```

Note, that both _dimension_ and _value_ parameters are mandatory. Then, in your Java view module, you should register preview widget in the cropper instnce:

```
    @UiField
    GWTCropper cropper;

    @UiField
    GWTCropperPreview cropperPreview;

    //somewhere, for example, in a constructor...
    this.cropper.registerPreviewWidget(this.cropperPreview);
```