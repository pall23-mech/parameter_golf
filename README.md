Compute Credit Request — $500
Páll Rúnarsson ENG, Reasearcher at Reykjavík University
I've built a working Parameter Golf pipeline (U-Net transformer + Muon optimizer + int6+QAT+zstd compression, 13.5× size reduction) and have results showing clear scaling on my RTX 5060 8GB:

10M param model → 1.90 BPB (400 steps)
22M param model → 1.508 BPB, 5.9GB VRAM used (card maxed out)

The target config (MODEL_DIM=768, NUM_LAYERS=12, TRAIN_SEQ_LEN=2048, 20k steps) OOMs locally and would take ~16 hours anyway. The competition has a 10-minute wallclock cap — I need H200 throughput to compete.
I request $500 in H200 credits to cover ~5–10 full competition runs. Pipeline is ready to go.

Decoder-only transformer, 22M parameters. 10 layers, 640 model dim, 8 attention heads (4 KV heads via GQA), 1024 sequence length, relu² MLP.

NUM_LAYERS=10 MODEL_DIM=640 NUM_HEADS=8 NUM_KV_HEADS=4 MLP_MULT=2 TRAIN_SEQ_LEN=1024 TRAIN_BATCH_TOKENS=131072
22M parameters, 5.9GB VRAM, 2840ms/step, achieves 1.508 BPB at 1000 steps — roughly 210 steps possible within the 10-minute competition wallclock on an 8gb 5060 blackwell.

The model reaches 1.51 BPB at 1000 steps, with stable convergence and no signs of saturation. Preliminary compression experiments achieve a 3.7× size reduction (6mb's), though further work is needed to preserve accuracy after quantization.
