## MuseMorphose, A Transformer-based VAE

* We develop the **in-attention** mechanism to firmly control Transformer decoders with _segment-level_, _dense_ conditions. 
* We then bridge the full song-level **in-attention** decoder and a _bar-wise_ Transformer encoder to construct our **MuseMorphose** model. 
* Trained with the VAE objective alone, **MuseMorphose** can perform style transfer on long musical pieces, while allowing users to control musical attributes down to the bar level.

## Paper
* Shih-Lun Wu, Yi-Hsuan Yang  
**_MuseMorphose_: Full-Song and Fine-Grained Music Style Transfer with Just One Transformer VAE**  
_ArXiv preprint_, May 2021  
[[arXiv](.) (coming soon)] [[code](.) (coming soon)] [[BibTex](.) (coming soon)]

## Brief Technical Overview
<div style="text-align:center;margin-top:6px;margin-bottom:6px">
  <figure>
    <img src="./assets/muse_morphose_archi.jpg" alt="model architecture" style="width:90%">
    <div style="font-size:0.9rem;opacity:0.8;margin-top:2px">
      <figcaption>Model architecture of MuseMorphose</figcaption>
    </div>
  </figure>
</div>

**MuseMorphose** is trained on expressive pop piano performances (dataset: _AILabs.tw-Pop1.7K_, [link](https://github.com/YatingMusic/compound-word-transformer/tree/main/dataset)).
We adopt a slightly revised REMI (_Huang and Yang, 2020_, [paper](https://arxiv.org/abs/2002.00212)) representation to convert the music into token sequences.

### 

## Listening Samples 
### 8-bar Excerpt #1  

| &bull; Original (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/excerpt01_orig.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation #1, **high** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt01_high.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #2, **low &rarr; high** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt01_crescendo.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #3, **high &rarr; low** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt01_diminuendo.mp3" type="audio/mpeg"></audio> |


### 8-bar Excerpt #2  

| &bull; Original (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/excerpt02_orig.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation #1, **low** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt02_low.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #2, **high** rhythm, **low &rarr; high** polyphony | <audio controls><source src="./assets/audio_samples/excerpt02_poly_crescendo.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #3, **high &rarr; low** rhythm, **high** polyphony | <audio controls><source src="./assets/audio_samples/excerpt02_rhym_diminuendo.mp3" type="audio/mpeg"></audio> |
