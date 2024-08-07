# GSAP

This repository showcases two interactive projects using GSAP (GreenSock Animation Platform): a Canvas Scroller and a Landing Page animation. Both projects demonstrate different aspects of GSAP for smooth, high-performance animations.

## Projects

### 1. Canvas Scroller

The Canvas Scroller project demonstrates how to create a smooth scrolling animation using a sequence of images rendered on a canvas. The animation synchronizes with the user's scroll, providing an engaging visual experience.

**Features:**
- **Image Preloading:** Loads a sequence of images for smooth scrolling.
- **Canvas Resizing:** Adjusts the canvas size dynamically based on the window's dimensions.
- **Animation Control:** Uses GSAP and ScrollTrigger to animate through the image sequence based on scroll position.

**JavaScript Code:**
```javascript
const canvas = document.querySelector('canvas');
const context = canvas.getContext('2d');
const frames = {
    currentIndex: 0,
    maxIndex: 382
};

let imagesLoaded = 0;
const images = [];

function preloadImages() {
    for (let i = 1; i <= frames.maxIndex; i++) {
        const imageUrl = `./frames/frame_${i.toString().padStart(4, "0")}.jpg`;
        const img = new Image();
        img.src = imageUrl;
        img.onload = () => {
            imagesLoaded++;
            if (imagesLoaded === frames.maxIndex) {
                loadImage(frames.currentIndex);
                startAnimation();
            }
        };
        images.push(img);
    }
}

function loadImage(index) {
    if (index >= 0 && index <= frames.maxIndex) {
        const img = images[index];
        if (!img) {
            console.error(`Image at index ${index} is not loaded yet.`);
            return;
        }
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        const scaleX = canvas.width / img.width;
        const scaleY = canvas.height / img.height;
        const scale = Math.max(scaleX, scaleY);

        const newWidth = img.width * scale;
        const newHeight = img.height * scale;

        const offsetX = (canvas.width - newWidth) / 2;
        const offsetY = (canvas.height - newHeight) / 2;

        context.clearRect(0, 0, canvas.width, canvas.height);
        context.imageSmoothingEnabled = true;
        context.imageSmoothingQuality = "high";
        context.drawImage(img, offsetX, offsetY, newWidth, newHeight);
        frames.currentIndex = index;
    }
}

function startAnimation() {
    var tl = gsap.timeline({
        scrollTrigger: {
            trigger: ".parent",
            start: "top top",
            scrub: 2,
            end: "bottom bottom",
        }
    });
    tl.to(frames, {
        currentIndex: frames.maxIndex,
        onUpdate: function () {
            loadImage(Math.floor(frames.currentIndex));
        }
    });
}

preloadImages();
```

### 2. Landing Page
The Landing Page Animation project showcases various entrance animations for elements on a landing page. It utilizes GSAP to animate elements on page load and on scroll for a more dynamic user experience.

**Features**
- **Page Load Animations:** Animates navigation elements and main content sections as they come into view.
- **Scroll Animations:** Adds animations to specific sections triggered by scroll position.

**JavaScript Code:**
```javascript
function page1Animation() {
    var tl = gsap.timeline();

    tl.from("nav h1, nav h4, nav button", {
        y: -40,
        opacity: 0,
        delay: 0.5,
        duration: 1,
        stagger: 0.15
    });

    tl.from(".center-part1 h1", {
        x: -300,
        opacity: 0,
        duration: 0.5
    });

    tl.from(".center-part1 p", {
        x: -100,
        opacity: 0,
        duration: 0.4
    });

    tl.from(".center-part1 button", {
        opacity: 0,
        duration: 0.4
    });

    tl.from(".center-part2 img", {
        opacity: 0,
        duration: 0.5,
        x: 200
    }, "-=0.7");

    tl.from(".section1bottom", {
        opacity: 0,
        y: 30,
        stagger: 0.15,
        duration: 0.5
    });
}

page1Animation();

function page2Animation() {
    var tl2 = gsap.timeline({
        scrollTrigger: {
            trigger: ".section2",
            scroller: "body",
            start: "top 50%",
            end: "top 0",
            scrub: 2
        }
    });

    tl2.from(".services", {
        y: 30,
        opacity: 0,
        duration: 0.5
    });

    tl2.from(".elem.line1.left", {
        x: -300,
        opacity: 0,
        duration: 1
    }, "anim1");

    tl2.from(".elem.line1.right", {
        x: 300,
        opacity: 0,
        duration: 1
    }, "anim1");

    tl2.from(".elem.line2.left", {
        x: -300,
        opacity: 0,
        duration: 1
    }, "anim2");

    tl2.from(".elem.line2.right", {
        x: 300,
        opacity: 0,
        duration: 1
    }, "anim2");
}

page2Animation();
```

## Getting Started
1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/GSAP.git
```
2. **Navigate to the project directory:**
```bash
cd GSAP
```
3. Open the HTML files in your browser to see the animations in action.

## Requirements
- GSAP (GreenSock Animation Platform)
- Modern web browser with JavaScript enabled
