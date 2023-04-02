<script setup>
import { ref } from 'vue'

const inputText = ref('');
const generatedImage = ref(null);
const selectedFont = ref('Lora'); // Valor predeterminado
const fontColor = ref('#000000'); // Color predeterminado
const bookAndAuthor = ref(''); // Valor predeterminado
const fontSize = ref(48); // Tamaño de fuente predeterminado
const fonts = [
  'Lora',
  'Roboto Slab',
  'Merriweather',
  'Playfair Display',
  'Source Serif Pro',
];

function getRandomColor() {
  const letters = '0123456789ABCDEF';
  let color = '#';
  for (let i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}

const generateImage = () => {
  const canvas = document.createElement('canvas');
  const ctx = canvas.getContext('2d');
  const width = 1200;
  const height = 900;
  canvas.width = width;
  canvas.height = height;

  // Crear el fondo degradado
  const gradient = ctx.createLinearGradient(0, 0, width, height);
  gradient.addColorStop(0, getRandomColor());
  gradient.addColorStop(1, getRandomColor());
  ctx.fillStyle = gradient;
  ctx.fillRect(0, 0, width, height);

  // Dibujar el texto en el centro del canvas
  ctx.fillStyle = fontColor.value; // Utilizar el color seleccionado
  ctx.font = `${fontSize.value}px ${selectedFont.value}`; // Utilizar la fuente y tamaño seleccionados
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  const textX = width / 2;
  const textY = height / 2;
  ctx.fillText(inputText.value, textX, textY);

    // Dibujar las comillas grandes alrededor de la cita
  const quoteSize = 80;
  ctx.font = `${quoteSize}px ${selectedFont.value}`;
  const quoteOpeningX = textX - ctx.measureText(inputText.value).width / 2 - quoteSize;
  const quoteOpeningY = textY - quoteSize / 2;
  const quoteClosingX = textX + ctx.measureText(inputText.value).width / 2;
  const quoteClosingY = textY + quoteSize / 2;
  ctx.fillText('“', quoteOpeningX, quoteOpeningY);
  ctx.fillText('”', quoteClosingX, quoteClosingY);

  // Dibujar el nombre del libro y el autor en la parte inferior derecha del canvas
  ctx.font = `24px ${selectedFont.value}`; // Utilizar una fuente más pequeña
  ctx.textAlign = 'right';
  ctx.textBaseline = 'bottom';
  ctx.fillText(`- ${bookAndAuthor.value}`, width - 20, height - 20); // Agregar un margen de 20px


  // Convertir el canvas a una imagen en formato base64
  const dataUrl = canvas.toDataURL('image/png');
  generatedImage.value = dataUrl;  
}

const shareOnTwitter = () => {
  // Aquí va el código para compartir la imagen en Twitter.
}

</script>

<template>
  <header class="bg-primary text-white py-4 mb-5">
    <div class="container text-center">
      <h1 class="display-5">GENERADOR DE CITAS A TWITTER</h1>
      <p class="lead">@lfer31 y chatGPT (Luis Fernando Muñoz) with ❤️</p>
    </div>
  </header>
  <div class="container mt-5">
    <div class="row">
      <div class="col">
        <textarea class="form-control" v-model="inputText" placeholder="Escribe algo aquí..." rows="6"></textarea>
      </div>
    </div>
    <!-- Agrega esto después del input del color de la fuente y antes de los botones -->
    <div class="row mt-3">
      <div class="col">
        <label for="bookAndAuthor">Nombre del libro y autor:</label>
        <input type="text" class="form-control" id="bookAndAuthor" v-model="bookAndAuthor">
      </div>
    </div>
    <div class="row mt-3">
      <div class="col">
        <label for="fontSelect">Selecciona una fuente:</label>
        <select class="form-select" id="fontSelect" v-model="selectedFont">
          <option v-for="font in fonts" :key="font" :value="font">{{ font }}</option>
        </select>
      </div>
      <div class="col">
        <label for="fontSize">Tamaño de la fuente para la cita: ({{ fontSize }}px)</label>
        <input type="range" class="form-range" id="fontSize" v-model="fontSize" min="20" max="100">
      </div>      
      <div class="col">
        <label for="fontColor">Selecciona el color de la fuente:</label>
        <input type="color" class="form-control form-control-color" id="fontColor" v-model="fontColor">
      </div>
    </div>
    <div class="row mt-3">
      <div class="col">
        <button class="btn btn-primary me-2" @click="generateImage">
          <i class="bi bi-image"></i> Generar cita
        </button>
        <button type="button" class="btn btn-primary" @click="shareOnTwitter">
          <i class="bi bi-twitter"></i> Compartir en Twitter
        </button>
      </div>
    </div>
    <div class="row mt-3" v-if="generatedImage">
      <div class="col">
        <img :src="generatedImage" alt="Generated Image" class="img-fluid">
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
