## MuseMorphose, A Transformer-based VAE

* We develop the **in-attention** mechanism to firmly control Transformer decoders with _segment-level_, _dense_ conditions. 
* We then bridge the full song-level **in-attention** decoder and a _bar-wise_ Transformer encoder to construct our **MuseMorphose** model. 
* Trained with the VAE objective alone, **MuseMorphose** can perform style transfer on long musical pieces, while allowing users to control musical attributes down to the bar level.

## Paper
* Shih-Lun Wu, Yi-Hsuan Yang  
**_MuseMorphose_: Full-Song and Fine-Grained Piano Music Style Transfer with One Transformer VAE**  
accepted to _IEEE/ACM Trans. Audio, Speech, & Language Processing (TASLP)_, Dec 2022  
[[paper](https://arxiv.org/abs/2105.04090){:target="_blank"}] [[supplement](./assets/supplement.pdf){:target="_blank"}] [[code](https://github.com/YatingMusic/MuseMorphose){:target="_blank"}] [[BibTex](https://drive.google.com/file/d/1EOpRjNqzIodNygxFVDJ-otY3BK-f56mr/view?usp=sharing){:target="_blank"}]

## Brief Technical Overview
<div style="text-align:center;margin-top:6px;margin-bottom:6px">
  <figure>
    <img src="./assets/muse_morphose_archi.jpg" alt="model architecture" style="width:90%">
    <div style="font-size:0.9rem;opacity:0.8;margin-top:2px">
      <figcaption>Model architecture of MuseMorphose</figcaption>
    </div>
  </figure>
</div>

**MuseMorphose** is trained on expressive pop piano performances (_AILabs.tw-Pop1.7K_ dataset, [link](https://github.com/YatingMusic/compound-word-transformer/tree/main/dataset){:target="_blank"}).
A slightly revised Revamped MIDI representation (**REMI**, _Huang and Yang, 2020_, [paper](https://arxiv.org/abs/2002.00212){:target="_blank"}) is adopted to convert the music into token sequences.

### Controllable Attributes
We consider the following two _computable_, _bar-level_ attributes:
* **Rhythmic intensity**: The percentage of quarter beats with _&ge;1 note onsets_.
* **Polyphony**: The average number of _notes hit or held_ on each quarter beat.  

Following _Kawai et al. (2020)_ ([paper](https://archives.ismir.net/ismir2020/paper/000099.pdf){:target="_blank"}), for each attribute, we assign each bar an ordinal class from $$[0, 1, \dots, 7]$$ according to the computed raw score.
The attributes are turned into learnable _attribute embeddings_

$$ \boldsymbol{a}^{\text{rhym}}, \boldsymbol{a}^{\text{poly}} \in \mathbb{R}^{d_{\boldsymbol{a}}} $$

fed to the decoder through **in-attention** to control the generation.

We note that more attributes can be potentially included, such as _rhythmic variation_ (ordinal), or _composing styles_ (nominal), just to name a few.

### In-attention Conditioning
To maximize the influence of bar-level conditions (i.e., $$\boldsymbol{c}_k$$'s) on the decoder, we inject them into _all_ $$L$$ self-attention layers via vector summation:

$$\begin{alignat}{2}
  \tilde{\boldsymbol{h}^l_t} &= \boldsymbol{h}^l_t + {\boldsymbol{c}_k}^{\top} W_{\text{in}} \,, \; \; \; \; &\forall \, l \in \{0, \dots, L-1\} \; \text{and} \; \forall \, t \in I_k \, \\
  \boldsymbol{c}_k &= \text{concat}([\boldsymbol{z}_k, \boldsymbol{a}^{\text{rhym}}_k, \boldsymbol{a}^{\text{poly}}_k])\, ,  &\forall \, k \in \{1, \dots, K\} \, ,
\end{alignat}$$

where $$W_{\text{in}}$$ is a learnable projection, $$I_k$$ stores the timestep indices for the $$k^{\text{th}}$$ bar, and $$\tilde{\boldsymbol{h}^l_t}$$'s are the _modified hidden states_ of layer $$l$$.

This mechanism promotes tight control by constantly reminding the model of the conditions' presence.

## Listening Samples
The samples demonstrate that **MuseMorphose** attains high _fidelity_ to the original song, strong _attribute control_, good _diversity_ across generations, and excellent _musicality_, all at the same time.

### 8-bar Excerpt #1  

| &bull; Original (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/excerpt01_orig.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation #1, **high** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt01_high.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #2, **ascending** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt01_crescendo.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #3, **descending** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt01_diminuendo.mp3" type="audio/mpeg"></audio> |


### 8-bar Excerpt #2  

| &bull; Original (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/excerpt02_orig.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation #1, **low** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/excerpt02_low.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #2, **high** rhythm, **ascending** polyphony | <audio controls><source src="./assets/audio_samples/excerpt02_poly_crescendo.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #3, **descending** rhythm, **high** polyphony | <audio controls><source src="./assets/audio_samples/excerpt02_rhym_diminuendo.mp3" type="audio/mpeg"></audio> |

### 8-bar Excerpt #3 (Mozart's "Ah vous dirai-je, Maman")

| &bull; Original (**theme melody** only) | <audio controls><source src="./assets/audio_samples/stars.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation #1, **ascending** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/stars_gen.mp3" type="audio/mpeg"></audio> |

### Full Song #1  

| &bull; Original (121 bars) | <audio controls><source src="./assets/audio_samples/song01_orig.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation, **increased** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/song01_increased.mp3" type="audio/mpeg"></audio> |

### Full Song #2  

| &bull; Original (87 bars) | <audio controls><source src="./assets/audio_samples/song02_orig.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation, **increased** rhythm, **decreased** polyphony | <audio controls><source src="./assets/audio_samples/song02_inc_dec.mp3" type="audio/mpeg"></audio> |


## Baseline Samples
Below we provide some compositions by RNN-based models  

### MIDI-VAE (Brunner et al., 2018)

| &bull; Original #1 (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/midi_vae_orig_01.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation #1, **low** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/midi_vae_gen_01.mp3" type="audio/mpeg"></audio> |
| &bull; Original #2 (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/midi_vae_orig_02.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #2, **high** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/midi_vae_gen_02.mp3" type="audio/mpeg"></audio> |

### Attributes-aware VAE (Kawai et al., 2020)

| &bull; Original #1 (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/attr_vae_orig_01.mp3" type="audio/mpeg"></audio> |
| --- | ----------- |
| &bull; Generation #1, **low** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/attr_vae_gen_01.mp3" type="audio/mpeg"></audio> |
| &bull; Original #2 (**mid** rhythm & polyphony) | <audio controls><source src="./assets/audio_samples/attr_vae_orig_02.mp3" type="audio/mpeg"></audio> |
| &bull; Generation #2, **high** rhythm & polyphony | <audio controls><source src="./assets/audio_samples/attr_vae_gen_02.mp3" type="audio/mpeg"></audio> |

## Authors and Affiliations
* **Shih-Lun Wu**  
  Research Intern @ _Taiwan AI Labs_ / Senior CS Major Undergrad @ _National Taiwan University_  
  b06902080@csie.ntu.edu.tw  
  [[website](https://slseanwu.github.io/){:target="_blank"}]
* **Yi-Hsuan Yang**  
  Chief Music Scientist @ _Taiwan AI Labs_ / Associate Research Fellow @ _Academia Sinica_  
  affige@gmail.com, yhyang@ailabs.tw  
  [[website](http://mac.citi.sinica.edu.tw/~yang/){:target="_blank"}]
  
<div style="display:flex;align-items:center;justify-content:space-around">
  <img src="./assets/AI-Labs-Logo-1-300x92.png" alt="Taiwan AI Labs" style="width:25%">
  <img src="./assets/ntu_logo.png" alt="NTU" style="width:33%">
  <img src="./assets/as_logo.png" alt="Academia Sinica" style="width:16%">
</div>
