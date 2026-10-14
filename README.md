# on-device-edge-llm
This repo store the usable Edge LLM models including its inference engines/runtimes that i've tried on my Android phone (Adreno 605, Snapdragon 865, 8GB of RAM, Android 12).

# Inference runtimes
The inference runtimes that worked well are:
- Ollama/llama.cpp in Termux
- Litert-lm python in Termux.
- MNN in ServLLama app.

# Workaround
On my device, which is Adreno 605, the most working GPU backend is OpenCL with CPU to GPU translation. Not the GPU model variant which most likely to output garbage characters even with the low temperature and top_p, top_k params.
The CPU backend is of course works perfectly well.
The WebGPU wllama (GGUF) works but mostly usable with gpu set to less than 20 or CPU only.
Keep in mind that the Quantization, KV Cache and the Parameter are crucial.
The less params mean the faster it get to respond (including vision image elaboration) but the less smarter it is. The higher params mean the smarter it is but the heavy it is to laods. The most efficient params tested on my device is 2-3B.
The best optimanl Quantization is Q4_K_M (int 4). Increasing the Quantization like to 6 (Q6_K), 8 or BF16 can increase the precision but heavy on loading.
And the KV Cache is depends on your RAM and GPU VRAM. For me it's 12287. You can try increasing it from 1024 up to the highest possible value.
The model file will be loaded to the RAM, so the bigger the size, the more RAM it will occupy.

Ollama even with OpenCL or Vulkan backend packages in Termux can't run the model. So for me i stucked with CPU.
Litert-lm and ServLLama MNN are the best inference runtime/runner to use the GPU using OpenCL.

# Models
Several models even with high params (let alone low params like 330M, 550M, 0.6B, 0.8B, etc) can't run pretty well by mean its understanding to the user context.

Some models that i've tried are:
- Gemma 4 E2B multi modal/omni (recommended)
- MiniCPM5
- MiniCPM V 4.6
- Qwen3.8 (including the VL variant)
- Qwen3.5 (including the VL variant)
- Qwen3 (including the VL variant)
- Qwen2.5 (including the VL variant)
- LFM2.5
- Deepseek R1
- SmolLM3
- Ministral 3
- Phi 4 mini reasoning

You'll need higher context window for better model tasks performance.
The KV Cache is used for all things including system&user prompt, document, conversation (user&model respond and thought process), and tool call.
You're most likely would like to create an agent for each inference runtimes (litert-lm, mnn, llama-cpp).
The agent is used to pass your prompt (system&user, document), conversation history (including tool call result), etc.
I've created that for uses in Ollama/llama-cpp, ServLLama MNN, and litert-lm through the use of its API endpoint.
