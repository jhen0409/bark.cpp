# bark.cpp

![bark.cpp](./assets/banner.jpeg)

[![Actions Status](https://github.com/PABannier/bark.cpp/actions/workflows/build.yml/badge.svg)](https://github.com/PABannier/bark.cpp/actions)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

[Roadmap](https://github.com/users/PABannier/projects/1) / [encodec.cpp](https://github.com/PABannier/encodec.cpp) / [ggml](https://github.com/ggerganov/ggml)

Inference of [SunoAI's bark model](https://github.com/suno-ai/bark) in pure C/C++.

**Disclaimer: there remains bug in the inference code, bark is able to generate audio for some prompts or some seeds,
but it does not work for most prompts. The current effort of the community is to fix those bugs, in order to release
v0.0.2**.

## Description

The main goal of `bark.cpp` is to synthesize audio from a textual input with the [Bark](https://github.com/suno-ai/bark) model in efficiently using only CPU.

- [X] Plain C/C++ implementation without dependencies
- [X] AVX, AVX2 and AVX512 for x86 architectures
- [X] Mixed F16 / F32 precision
- [X] 4-bit, 5-bit and 8-bit integer quantization
- [ ] Optimized via ARM NEON, Accelerate and Metal frameworks
- [ ] iOS on-device deployment using CoreML

The original implementation of `bark.cpp` is the bark's 24Khz English model. We expect to support multiple encoders in the future (see [this](https://github.com/PABannier/bark.cpp/issues/36) and [this](https://github.com/PABannier/bark.cpp/issues/6)), as well as music generation model (see [this](https://github.com/PABannier/bark.cpp/issues/62)). This project is for educational purposes.

**Supported platforms:**

- [X] Mac OS
- [X] Linux
- [X] Windows

**Supported models:**

- [X] Bark
- [ ] Vocos
- [ ] AudioCraft

---

Here is a typical run using Bark:

```java
make -j && ./main -p "this is an audio"
I bark.cpp build info:
I UNAME_S:  Darwin
I UNAME_P:  arm
I UNAME_M:  arm64
I CFLAGS:   -I. -O3 -std=c11   -fPIC -DNDEBUG -Wall -Wextra -Wpedantic -Wcast-qual -Wdouble-promotion -Wshadow -Wstrict-prototypes -Wpointer-arith -Wmissing-prototypes -pthread -DGGML_USE_ACCELERATE
I CXXFLAGS: -I. -O3 -std=c++11 -fPIC -DNDEBUG -Wall -Wextra -Wpedantic -Wcast-qual -Wno-unused-function -Wno-multichar -pthread
I LDFLAGS:   -framework Accelerate
I CC:       Apple clang version 14.0.0 (clang-1400.0.29.202)
I CXX:      Apple clang version 14.0.0 (clang-1400.0.29.202)

bark_model_load: loading model from './ggml_weights'
bark_model_load: reading bark text model
gpt_model_load: n_in_vocab  = 129600
gpt_model_load: n_out_vocab = 10048
gpt_model_load: block_size  = 1024
gpt_model_load: n_embd      = 1024
gpt_model_load: n_head      = 16
gpt_model_load: n_layer     = 24
gpt_model_load: n_lm_heads  = 1
gpt_model_load: n_wtes      = 1
gpt_model_load: ggml tensor size = 272 bytes
gpt_model_load: ggml ctx size = 1894.87 MB
gpt_model_load: memory size =   192.00 MB, n_mem = 24576
gpt_model_load: model size  =  1701.69 MB
bark_model_load: reading bark vocab

bark_model_load: reading bark coarse model
gpt_model_load: n_in_vocab  = 12096
gpt_model_load: n_out_vocab = 12096
gpt_model_load: block_size  = 1024
gpt_model_load: n_embd      = 1024
gpt_model_load: n_head      = 16
gpt_model_load: n_layer     = 24
gpt_model_load: n_lm_heads  = 1
gpt_model_load: n_wtes      = 1
gpt_model_load: ggml tensor size = 272 bytes
gpt_model_load: ggml ctx size = 1443.87 MB
gpt_model_load: memory size =   192.00 MB, n_mem = 24576
gpt_model_load: model size  =  1250.69 MB

bark_model_load: reading bark fine model
gpt_model_load: n_in_vocab  = 1056
gpt_model_load: n_out_vocab = 1056
gpt_model_load: block_size  = 1024
gpt_model_load: n_embd      = 1024
gpt_model_load: n_head      = 16
gpt_model_load: n_layer     = 24
gpt_model_load: n_lm_heads  = 7
gpt_model_load: n_wtes      = 8
gpt_model_load: ggml tensor size = 272 bytes
gpt_model_load: ggml ctx size = 1411.25 MB
gpt_model_load: memory size =   192.00 MB, n_mem = 24576
gpt_model_load: model size  =  1218.26 MB

bark_model_load: reading bark codec model
encodec_model_load: model size    =   44.32 MB

bark_model_load: total model size  =    74.64 MB

bark_generate_audio: prompt: 'this is an audio'
bark_generate_audio: number of tokens in prompt = 513, first 8 tokens: 20579 20172 20199 33733 129595 129595 129595 129595
bark_forward_text_encoder: ...........................................................................................................

bark_forward_text_encoder: mem per token =     4.80 MB
bark_forward_text_encoder:   sample time =     7.91 ms
bark_forward_text_encoder:  predict time =  2779.49 ms / 7.62 ms per token
bark_forward_text_encoder:    total time =  2829.35 ms

bark_forward_coarse_encoder: .................................................................................................................................................................
..................................................................................................................................................................

bark_forward_coarse_encoder: mem per token =     8.51 MB
bark_forward_coarse_encoder:   sample time =     3.08 ms
bark_forward_coarse_encoder:  predict time = 10997.70 ms / 33.94 ms per token
bark_forward_coarse_encoder:    total time = 11036.88 ms

bark_forward_fine_encoder: .....

bark_forward_fine_encoder: mem per token =     5.11 MB
bark_forward_fine_encoder:   sample time =    39.85 ms
bark_forward_fine_encoder:  predict time = 19773.94 ms
bark_forward_fine_encoder:    total time = 19873.72 ms



bark_forward_encodec: mem per token = 760209 bytes
bark_forward_encodec:  predict time =   528.46 ms / 528.46 ms per token
bark_forward_encodec:    total time =   663.63 ms

Number of frames written = 51840.


main:     load time =  1436.36 ms
main:     eval time = 34520.53 ms
main:    total time = 35956.92 ms
```

## Usage

Here are the steps for the bark model.

### Get the code

```bash
git clone https://github.com/PABannier/bark.cpp.git
cd bark.cpp
```

### Build

In order to build bark.cpp you have two different options. We recommend using `CMake` for Windows.

- Using `make`:
  - On Linux or MacOS:

      ```bash
      make
      ```

- Using `CMake`:

    ```bash
    mkdir build
    cd build
    cmake ..
    cmake --build . --config Release
    ```

### Prepare data & Run

```bash
# install Python dependencies
python3 -m pip install -r requirements.txt

# obtain the original bark and encodec weights and place them in ./models
python3 download_weights.py --download-dir ./models

# convert the model to ggml format
python3 convert.py \
        --dir-model ./models \
        --codec-path ./models \
        --vocab-path ./models \
        --out-dir ./ggml_weights/

# run the inference
./main -m ./ggml_weights/ -p "this is an audio"
```

### Optional quantize weights

Weights can be quantized using the following strategy: `q4_0`, `q4_1`, `q5_0`, `q5_1`, `q8_0`.

```bash
./quantize ./ggml_weights/ggml_weights_text.bin ./ggml_weights_q4/ggml_weights_text.bin q4_0
./quantize ./ggml_weights/ggml_weights_coarse.bin ./ggml_weights_q4/ggml_weights_coarse.bin q4_0
./quantize ./ggml_weights/ggml_weights_fine.bin ./ggml_weights_q4/ggml_weights_fine.bin q4_0
./quantize ./ggml_weights/ggml_weights_codec.bin ./ggml_weights_q4/ggml_weights_codec.bin q4_0
```

### Seminal papers and background on models

- Bark
    - [Text Prompted Generative Audio](https://github.com/suno-ai/bark)
- Encodec
    - [High Fidelity Neural Audio Compression](https://arxiv.org/abs/2210.13438)
- GPT-3
    - [Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165)

### Contributing

`bark.cpp` is a continuous endeavour that relies on the community efforts to last and evolve. Your contribution is welcome and highly valuable. It can be

- bug report: you may encounter a bug while using `bark.cpp`. Don't hesitate to report it on the issue section.
- feature request: you want to add a new model or support a new platform. You can use the issue section to make suggestions.
- pull request: you may have fixed a bug, added a features, or even fixed a small typo in the documentation, ... you can submit a pull request and a reviewer will reach out to you.

### Coding guidelines

- Avoid adding third-party dependencies, extra files, extra headers, etc.
- Always consider cross-compatibility with other operating systems and architectures
- Avoid fancy looking modern STL constructs, keep it simple
- Clean-up any trailing whitespaces, use 4 spaces for indentation, brackets on the same line, `void * ptr`, `int & ref`
