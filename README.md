<div align="center">
  <div>
    <h1 align="center">NSFW Filter</h1>
  </div>
	<p>An npm library that helps filter out inappropriate images using AI.

<a href="https://www.npmjs.com/package/nsfw-filter"><img src="https://img.shields.io/npm/v/nsfw-filter" alt="Current version"></a>

</div>

---

`nsfw-filter` is currently used in production to process hundreds of thousands of images for a popular image restoration service called <a href="https://www.restorephotos.io/">restorePhotos</a>. It helps prevent people from uploading inappropriate pictures.

## Installation

`npm i nsfw-filter`

## Usage

```
import NSFWPredictor from 'nsfw-filter';

const isSafe = await NSFWPredictor.isSafe(image)

```

## How it works

This library uses both Tensorflow.js, an OSS library for machine learning models, and `nsfwjs` to predict whether a given image is NSFW (Not Safe For Work).

It's currently being used in RestorePhotos, which is a popular open source projects with 250k monthly visitors. [See how it's used here.](https://github.com/Nutlope/restorePhotos/blob/main/pages/restore.tsx#L45)
