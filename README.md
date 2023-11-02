# RL-AWQ: RL-based Activation-aware Weight Quantization for LLM Compression and Acceleration 

The main phases performed in AWQ quantization method is as follows:

![overview](figures/AWQ.png)

The above architecture mainly consists of three phases:
- First create the required AWQ for the following quantization. This step can be performed on-the-fly or by loading the previous computed AWQ
- The scond step, involves calculating the quantization parameters for the weights and activations. This step has two main approachs: 1: Using a psudo quantization method which just quantize the wieghts and activations without considering a new model architecture. 2: Using a real quantization method which considers a new model architecture (i.e., WQLinear) besides the wights and activations quantization.
- The final step is to load the quntized model by dispatching the quantized weights and architectures (in real qunatization scenrario) by calling the ModelDispatch method. Subsequently, we can utlize LM evaluators such as LM_eval to evaluate the model on a specifiec specterum of tasks.

The main technical contributins reside in:
- Considering the fact that, given weight quantization task, not all weights matter the same. Some weights (i.e., salient weights) are more important than others. Therefore, we can first find these important weights and keep them while quantizing others.
- However such approach have mized-precision weights- the salient weights in FP16 and others in INT3 which is clearly not hardware friendly. To solve this technical issue, we can first scale the weights by a learnable value s and then quantize the weights. Such s will be found in a greedy search problem formulation in a way that the difference between quantized salienrt weights and the initial salient weights min9imize while the prepelixity of the LM stay the same.

The main quantization method they used is a zero-based Round to Nearest (RTN).