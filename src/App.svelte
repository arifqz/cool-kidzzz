<script>
    import { onMount, onDestroy } from 'svelte';
    import p5 from 'p5';
  
    let myp5;
    let useHeightStyle = true; // Set this to false to disable setting text height based on height style
  
    onMount(() => {
      myp5 = new p5(p => {
        let font;
        let img = { src: 'assets/your-image.jpg', position: 0.15, width: .70 }; // replace with your image file, position, and width
        let lines = ["CoolKidzzz.com", "", "IMHOTEP", "IS THE", "COOLEST", "KID"];
        let styles = [
  { color: '#ffffff', height: 0.4, width: .9, position: 0.05, shear: 0 }, // red
  { color: '#ffffff', height: 0.3, width: 1, position: 0.3, shear: 0 }, // space for JPG
  { color: '#ffffff', height: 0.35, width: 0.9, position: 0.50, shear: p.PI / 1 }, // green
  { color: '#ffffff', height: 0.3, width: 0.4, position: 0.60, shear: p.PI / 1 }, // blue
  { color: '#ffffff', height: 0.4, width: .9, position: 0.70, shear: p.PI / 1 }, // yellow
  { color: '#ffffff', height: 0.5, width: .9, position: 0.85, shear: p.PI / 1 } // white
];
  
        p.preload = () => {
          font = p.loadFont('assets/ArchivoBlack-Regular.ttf'); // with your font file
          img.src = p.loadImage(img.src);
        }
  
  
        p.setup = () => {
          let canvasHeight = p.windowHeight;
          let canvasWidth = (canvasHeight * 9) / 16; // Set the width based on the height to maintain a 9:16 aspect ratio
          p.createCanvas(canvasWidth, canvasHeight);
          p.textFont(font);
          p.textAlign(p.CENTER, p.CENTER);
        }
  
        p.windowResized = () => {
          let canvasHeight = p.windowHeight;
          let canvasWidth = (canvasHeight * 9) / 16; // Set the width based on the height to maintain a 9:16 aspect ratio
          p.resizeCanvas(canvasWidth, canvasHeight);
        }
  
        p.draw = () => {
          p.background(0);
          lines.forEach((line, i) => {
            let style = styles[i];
            let textSize;
            if (useHeightStyle) {
              textSize = p.windowHeight * style.height; // text size is a fraction of window height if useHeightStyle is true
            } else {
              textSize = p.textWidth(line); // text size is based on text width if useHeightStyle is false
            }
            let y = p.windowHeight * style.position; // calculate y-coordinate based on window height

            // Set the text size
            p.textSize(textSize);

            // Calculate the width of the text
            let textWidth = p.textWidth(line);

            // Calculate the scale factors
            let scaleFactorX = (p.width * style.width) / textWidth;
            let scaleFactorY = style.height;

            // Save the current transformation matrix
            p.push();

            // Translate to the center of the canvas
            p.translate(p.width / 2, y);

            // Scale in x and y directions independently
            p.scale(scaleFactorX, scaleFactorY);

            // Shear the text
            p.shearX(style.shear);

            // Draw the text
            p.noStroke(); // no outline
            p.fill(p.color(style.color)); // set the font color
            p.text(line, 0, 0);

            p.pop();

            // Draw the image after "HAPPY BIRTHDAY"
            if (i === 0) {
              img.src.resize(p.width * img.width, 0); // resize the image to the specified fraction of the canvas width
              let x = (p.width - img.src.width) / 2; // calculate x-coordinate to center the image
              p.image(img.src, x, p.height * img.position);
            }
          });
        }
      }, 'canvas-container');
    });
  
    onDestroy(() => {
      myp5.remove();
    });
</script>
<style>
    #canvas-container {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    :global(body) {
        background-color: black;
    }
</style>
<div id="canvas-container"></div>