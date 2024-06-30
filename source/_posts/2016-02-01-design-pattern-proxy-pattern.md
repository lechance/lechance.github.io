---
title: Design Pattern - Proxy Pattern
date: 2016-02-01 13:15:42
categories:
- Design Pattern
tags:
- design pattern
- proxy
- java
---
>
Provide a surrogate or placeholder for another object to control access to it.


#### In Computer World Example

Consider an ATM implementation for a bank. Here we will find multiple proxy objects, Actual bank information will be stored in a remote server. We must remember that in te programming world, the creation of multiple instances of a complex object(heavy object) is very costly. In such situations, we can create multiple proxy object(which must point to an original object) and the total creation of actual objects can be carried out on a demand basis. Thu we can save both the memory and creational time.

#### Implementation
The figure below shows a `UML class diagram` for the `Proxy Pattern`.

![uml diagram] (/images/uml/proxy_pattern_uml_diagram.png "uml" )

The participants classes in the proxy pattern are:
- Subject - interface implemented by the RealSubject and representing its services. The interface must be implemented by the proxy as well so that the proxy can be used in any location where the RealSubject can be used.
- Proxy:
 - Maintains a reference that allows the Proxy to access the RealSubject.
 - Implements the same interface implemented by the RealSubject so that the Proxy can be subsituted for the RealSubject.
 - Controls access to the RealSubject and may be responsible for its creation and deletion.
 - Other responsibilities depend on the kind of proxy.
- RealSubject - the real object that the proxy repsents.

#### Applicability & Examples

The Proxy design pattern is applicable when there is a need to control access to an Object, as well as when there is a need for a sophisiticated reference to an Ojbect. Common Situations where the proxy pattern is applicable are:
- Virtual Proxies: delaying the creation and initialization of expensive objects util needed, where the objects are created on demand(For example creating the RealSubject object only when the doSomething method is invoked).
- Remote Proxies: providing a local representation for an object that is in a different address space. A common example is Java RMI stub objects. The stub object acts as proxy where invoking methods on the stub would cause the stub to communicate and invoke methds on a remote object(called skeleton) found on a different machine.
- Protection Proxies: where a proxy controls access to RealSubject methods, by giving access to some object while denying access to others.
- Smart Reference: providing a sophisticated access to certain objects such as tracking the number of references to an object and denying access if a certain number is reached, as well as loading an object from database into memory on demand.
#### Coding in Java

```java

/**
 * subject interface
 * <br />
 * The <code>Image</code> interface has a single method #displayImage() that
 * the concrete class must implement to render to an image to screen.
 */
interface Image {
    void displayImage();
}

/**
 * Proxy
 * <br />
 * the ImageProxy is a virtual proxy that creates and loads the actual image
 * object on demand, thus saving the cost of loading an image into memory
 * until it needs to be rendered.
 */
class ImageProxy implements Image {
    /**
     * Private Proxy data
     */
    private String imageFilePath;

    /**
     * Reference to RealSubject
     */
    private Image proxiedImage;

    public ImageProxy(String imageFilePath) {
        this.imageFilePath = imageFilePath;
    }

    @Override
    public void displayImage() {

        //Create the Image object only when the image is required to be shown
        if (proxiedImage == null) {
            proxiedImage = new HighResolutionImage(imageFilePath);
        }
        //now call displayImage on realSubject
        proxiedImage.displayImage();
    }
}

/**
 * Real Subject
 * <br />
 * the RealSubject Implementation,
 * which is the concrete and heavyweight implementation of the image interface.
 * The High resolution image, loads a high resolution image from disk,
 * and renders it to screen when showImage() is called.
 */
class HighResolutionImage implements Image {

    private String imageFilePath;

    public HighResolutionImage(String imageFilePath) {
        this.imageFilePath=imageFilePath;
        loadImage(imageFilePath);
    }

    private void loadImage(String imageFilePath){
        //load image from disk into memory
        //this is heavy and costly operation
        System.out.println("loading image from disk...:"+imageFilePath);
    }
    @Override
    public void displayImage() {
        //actual image rendering logic
        System.out.println("Show Image: -_- "+imageFilePath);
    }
}

/**
 * ProxyPatternDemo Created by lechance.
 */
public class ProxyPatternDemo {

    public static void main(String[] args) {

        //assuming that user selects a folder that has 3 images

        //create the 3 images
        Image highResolutionImage1 = new ImageProxy("images/pic1.jpeg");
        Image highResolutionImage2 = new ImageProxy("images/pic2.jpeg");
        Image highResolutionImage3 = new ImageProxy("images/pic3.jpeg");
        //assume that the user clicks on Image one item in a list
        //this would cause the program to call displayImage() for that image only
        //note that in this case only image one was loaded into memory
        highResolutionImage1.displayImage();

        System.out.println("\n-------------------\n");

        //consider using high resolution image object directly
        Image highResolutionImage1NoProxy = new HighResolutionImage("images/pc1.jpeg");
        Image highResolutionImage2NoProxy = new HighResolutionImage("images/pc2.jpeg");
        Image highResolutionImage3NoProxy = new HighResolutionImage("images/pc3.jpeg");
        //assume that the user selects image two item from  images list
        highResolutionImage2NoProxy.displayImage();
        //note that is this case all images have been loaded
        // into memory and not all have been actually displayed.
        //this is a waste of memory resources
    }
}


```

![proxy pattern](/images/Kazam_screenshot_proxy.png)

> Orignazie from network
