## MuseMorphose, A Transformer-based VAE

* We develop the **in-attention** mechanism to firmly control Transformer decoders with _segment-level_, _dense_ conditions. 
* We then bridge the full song-level **in-attention** decoder and a _bar-wise_ Transformer encoder to construct our **MuseMorphose** model. 
* Trained with the VAE objective alone, **MuseMorphose** can perform style transfer of long musical pieces, while allowing user controls on musical attributes down to the bar level.

## Paper
* Shih-Lun Wu, Yi-Hsuan Yang  
**_MuseMorphose_: Full-Song and Fine-Grained Music Style Transfer with Just One Transformer VAE**  
_ArXiv preprint_, May 2021  
[[arXiv](.) (coming soon)] [[code](.) (coming soon)] [[BibTex](.) (coming soon)]

## Model Bird's-eye View
<div style="text-align:center;margin-top:6px;margin-bottom:6px">
  <figure>
    <img src="./assets/muse_morphose_archi.jpg" alt="model architecture" style="width:90%">
    <div style="font-size:0.9rem;opacity:0.8;margin-top:2px">
      <figcaption>Model architecture of MuseMorphose</figcaption>
    </div>
  </figure>
</div>

## Listening Samples
**MuseMorphose** is trained on expressive pop piano performances (dataset: _AILabs.tw Pop17K_, [link](https://github.com/YatingMusic/compound-word-transformer/tree/main/dataset)). The following samples are 8 bars long, but we note that **MuseMorphose** can deal with arbitrarily long musical pieces.
