NestJS File Upload Service (Local & Cloudinary) 🚀
Este proyecto es un módulo / guía de implementación para la gestión de carga de archivos en NestJS. Permite almacenar archivos tanto de manera local en el servidor como en la nube utilizando Cloudinary, configurado de forma segura mediante variables de entorno (.env).

📁 Estructura del Almacenamiento
El directorio uploads/ se divide en dos entornos estratégicos:

Plaintext


uploads/
├── local/      # Archivos guardados localmente en el servidor
└── nube/       # Archivos procesados o temporales previa subida a Cloudinary
🛠️ Requisitos Previos
Asegúrate de instalar las dependencias necesarias para el manejo de archivos (Multer) y la integración con Cloudinary:

Bash


npm install --save @nestjs/platform-express multer
npm install --save cloudinary dotenv
npm install --save-dev @types/multer
⚙️ Configuración de Variables de Entorno (.env)
Crea un archivo .env en la raíz de tu proyecto y configura las credenciales necesarias:

Fragmento de código


# Puerto del servidor
PORT=3000

# Configuración de Cloudinary
CLOUDINARY_CLOUD_NAME=tu_cloud_name
CLOUDINARY_API_KEY=tu_api_key
CLOUDINARY_API_SECRET=tu_api_secret
🚀 Modos de Almacenamiento
1. Almacenamiento Local (uploads/local)
Ideal para entornos de desarrollo local o almacenamiento estático servido directamente por el servidor.

Carpeta de destino: uploads/local/

Uso recomendado: Fotos de perfil temporales, documentos locales, pruebas de desarrollo.

2. Almacenamiento en la Nube (uploads/nube + Cloudinary)
Utiliza la API de Cloudinary para subir, transformar y optimizar imágenes directamente en la nube.

Carpeta de soporte/temporal: uploads/nube/

Procesamiento: Los archivos se reciben en el controlador, se validan y se suben a tu cuenta de Cloudinary retornando la URL pública segura (https).

📝 Uso del Módulo en NestJS
Ejemplo de Controlador (uploads.controller.ts)
TypeScript


import { Controller, Post, UseInterceptors, UploadedFile } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { UploadsService } from './uploads.service';

@Controller('uploads')
export class UploadsController {
  constructor(private readonly uploadsService: UploadsService) {}

  // Subir archivo localmente
  @Post('local')
  @UseInterceptors(FileInterceptor('file'))
  uploadLocal(@UploadedFile() file: Express.Multer.File) {
    return this.uploadsService.saveLocal(file);
  }

  // Subir archivo a Cloudinary
  @Post('nube')
  @UseInterceptors(FileInterceptor('file'))
  uploadToCloud(@UploadedFile() file: Express.Multer.File) {
    return this.uploadsService.uploadToCloudinary(file);
  }
}