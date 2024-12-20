# LLaVA-1.5: Improved Reasoning with Efficient Training

## Overview
LLaVA-1.5 represents a significant advancement in vision-language models, achieving state-of-the-art performance across 11 benchmarks while maintaining remarkable training efficiency. The model introduces key architectural improvements including CLIP-ViT-L-336px with MLP projection and specialized VQA data training.

## Key Features
- Training Efficiency: Complete training in ~1 day on 8-A100 GPUs
- Data Efficiency: Achieves SOTA using only 1.2M publicly available samples
- Architecture: Leverages powerful fully-connected vision-language connector
- Resolution Handling: Explores scaling to higher resolution inputs
- Transparency: Provides detailed analysis of limitations and hallucination risks

## Technical Implementation
```python
# Example inference code using LLaVA-1.5
from llava.model import LlavaLlamaForCausalLM
from llava.conversation import conv_templates
from llava.utils import disable_torch_init
from llava.model.builder import load_pretrained_model

# Load model
model_path = "liuhaotian/llava-v1.5-13b"
model, tokenizer, image_processor, context_len = load_pretrained_model(
    model_path=model_path,
    model_base=None,
    model_name=model_path
)

# Process image and generate response
image = image_processor(image_path)
conv = conv_templates[conv_mode].copy()
conv.append_message(conv.roles[0], prompt)
conv.append_message(conv.roles[1], None)
prompt = conv.get_prompt()

# Generate response
inputs = tokenizer([prompt])
input_ids = torch.tensor(inputs.input_ids).cuda()
output_ids = model.generate(
    input_ids,
    images=image.unsqueeze(0).cuda(),
    max_new_tokens=max_new_tokens,
    temperature=temperature
)
Citation
@article{liu2023llava,
  title={LLaVA-1.5: Improved Reasoning, OCR, and World Knowledge in Language-Vision Assistants},
  author={Liu, Haotian and Li, Chunyuan and Li, Qingyang and Lee, Yong Jae},
  journal={arXiv preprint arXiv:2310.03744},
  year={2023}
}
