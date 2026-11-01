# AsymSeal: project page

**Live page:** https://yanghangai.github.io/AsymSeal/

AsymSeal is an invisible image watermark that carries 1,024 bits. A tiny network writes the bits, and a much larger one reads them back. This repository hosts the project page. The page covers:

- the task;
- the results on the test set;
- the method;
- a live demo that runs the released model in your browser.

## The demo runs on your machine

The page includes a JavaScript port of the released checkpoint (seed 0, at its deployed operating point). Your images are never uploaded.

- **GPU:** with WebGL2 the model runs on your GPU. At startup the page checks the GPU result against the CPU code and switches to the CPU if they disagree.
- **CPU:** without WebGL2 it runs on the CPU. Decoding a 512 × 512 canvas then takes about two minutes.
- **Signed payload:** this mode uses your browser's built-in Ed25519. The key is made in the page and stays there.
- **Through a real platform:** download the watermarked PNG, or share it to an app on a phone. Send it through a messenger or post it, then decode the copy you get back. The page keeps the payload of the last image you downloaded or shared in your browser's `localStorage`, so a copy can still be checked if the tab reloads. For a signed image this is the public key, not the private key. Nothing is sent anywhere.

The page works on phones. Touch drags the comparison slider and explores the carrier, and the wide diagrams are redrawn for narrow screens.

The demo differs from the paper's measurements in two ways:

- JPEG attacks use your browser's JPEG encoder, not the one used in the evaluation.
- The model runs in fp32, while the paper's tables used fp16 autocast.

So demo numbers will be close to the paper's but not identical. The page's results section shows the paper's numbers.

## Files

| File | Contents |
|---|---|
| `index.html` | The page, with all scripts inline. No external requests. |
| `model.json` | The tensor list, the deployed operating point, and the SHA-256 of the weights and of the source checkpoint. |
| `weights.b64.txt` | The float32 weights (little-endian, 3,459,988 bytes), base64-encoded. |
| `ties.json` | The tie-break table that makes the carrier permutations match the PyTorch reference. |
| `samples/*.jpg` | The demo's sample photos (CC0, Wikimedia Commons), scaled to 2048 px on the long side. |

The JavaScript port was checked against the PyTorch release code on three images (128 × 128, 200 × 330 and 600 × 520). Every decoded bit sign agreed, and bit accuracy was identical. At most 2 of 198,000 8-bit pixel values differed, each by one level.

## Licence

The code and the model weights are released under CC BY-NC 4.0 (see `LICENSE`).

The 128-bit perceptual hash in the signed payload follows the PDQ image-hashing algorithm from Facebook's ThreatExchange (<https://github.com/facebook/ThreatExchange>). It is not bit-compatible with other PDQ implementations.

Sample photos, all CC0 via Wikimedia Commons:

- Jebulon: *Patio de los Arrayanes detail Alhambra Granada Spain*, *Courtyard Zappeion Athens, Greece*, *Carrelet, Esnandes, Charente-Maritime, august 2015*.
- David Marcu: *Alone in the unspoilt wilderness (Unsplash)*.
