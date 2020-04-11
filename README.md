# Type-Writter-Effect
This is a cool effect using JavaScript (vanilla)


HTML CODE
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="style.css">
  <link rel="stylesheet" href="https://fontawesome.com/">
  <title>Welcome To My Site</title>
</head>
<body>
     <div class="container">
    <h1>Alandis Seals The 
      <span class="txt-type" data-wait="3000" data-words='["Developer", "Designer", "Creator"]'></span>
    </h1>
    <h2>Welcome To My Website</h2>
    <nav>
        <ul> 
             <li class="current"><a href="index.html">Home</a></li>
             <li><a href="about.html">About</a></li>
             <li><a href="index.html">Projects</a></li>
             <li><a href="contact.html">contact</a></li>
         </ul>
     </nav>
  </div>
  

  <script src="sript.js"></script>
</body>
</html>


CSS CODE
@import url('https://fonts.googleapis.com/css?family=Raleway:200,100,400');

body {
  font-family: 'Raleway', sans-serif;
  height: 100vh;
  background: #333 url('https://image.ibb.co/n5A2HU/showcase.jpg') no-repeat center center / cover;
  color: #ccc;
  overflow:hidden;
}

ul{
    margin: 0;
    padding: 0;
    }

nav{
    float: right;
    margin-top: 50px;
}

a{
    color:white;
    text-decoration: none;
    text-transform: uppercase;
    font-size: 20px;
}


li{
    /* float: left; */
    display: inline;
    padding: 0 30px 0 30px;

}

.current a{
    color: #e8491d;
    font-weight: bold;
}

a:hover{
    color: #cccccc;
    font-weight: bold;

}
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
  padding: 0 3rem;
}

h1, h2 {
  font-weight: 200;
  margin: 0.4rem;
}

h1 {
  font-size: 3.5rem;
}

h2 {
  font-size: 2rem;
  color: #aaa;
}

/* Cursor */
.txt-type > .txt {
  border-right: 0.2rem solid #777;
}

@media(min-width: 1200px) {
  h1 {
    font-size: 5rem;
  }
}

@media(max-width: 800px) {
  .container {
    padding: 0 1rem;
  }

  h1 {
    font-size: 3rem;
  }
}

@media(max-width: 500px) {
  h1 {
    font-size: 2.5rem;
  }

  h2 {
    font-size: 1.5rem;
  }
}


JAVASCRIPT CODE
// ES6 Class
class TypeWriter {
    constructor(txtElement, words, wait = 3000) {
      this.txtElement = txtElement;
      this.words = words;
      this.txt = '';
      this.wordIndex = 0;
      this.wait = parseInt(wait, 10);
      this.type();
      this.isDeleting = false;
    }
  
    type() {
      // Current index of word
      const current = this.wordIndex % this.words.length;
      // Get full text of current word
      const fullTxt = this.words[current];
  
      // Check if deleting
      if(this.isDeleting) {
        // Remove char
        this.txt = fullTxt.substring(0, this.txt.length - 1);
      } else {
        // Add char
        this.txt = fullTxt.substring(0, this.txt.length + 1);
      }
  
      // Insert txt into element
      this.txtElement.innerHTML = `<span class="txt">${this.txt}</span>`;
  
      // Initial Type Speed
      let typeSpeed = 300;
  
      if(this.isDeleting) {
        typeSpeed /= 2;
      }
  
      // If word is complete
      if(!this.isDeleting && this.txt === fullTxt) {
        // Make pause at end
        typeSpeed = this.wait;
        // Set delete to true
        this.isDeleting = true;
      } else if(this.isDeleting && this.txt === '') {
        this.isDeleting = false;
        // Move to next word
        this.wordIndex++;
        // Pause before start typing
        typeSpeed = 500;
      }
  
      setTimeout(() => this.type(), typeSpeed);
    }
  }
  
  
  // Init On DOM Load
  document.addEventListener('DOMContentLoaded', init);
  
  // Init App
  function init() {
    const txtElement = document.querySelector('.txt-type');
    const words = JSON.parse(txtElement.getAttribute('data-words'));
    const wait = txtElement.getAttribute('data-wait');
    // Init TypeWriter
    new TypeWriter(txtElement, words, wait);
  }
