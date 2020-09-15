---
title: "工程笔记"
permalink: /wiki/engineering/
last_modified_at: 2020-08-11
toc: true
---

## 工程 

### 代码腐化问题  
- [程序的腐化原因及建议](https://cloud.tencent.com/developer/article/1159878)
- [聊聊程序腐化那些事](https://kknews.cc/tech/y62kjjk.html)


原文網址：https://kknews.cc/tech/y62kjjk.html]()
## C++

- [当析构函数遇到多线程](http://files.cppblog.com/Solstice/dtor_meets_mt.pdf)

## Linux  
- [Fix The Google GPG Error on Ubuntu](https://www.omgubuntu.co.uk/2017/08/fix-google-gpg-key-linux-repository-error)
- [CudaDockerFile](https://gitlab.com/nvidia/container-images/cuda/-/tree/master/dist/ubuntu18.04/10.1)
- [What Is /dev/shm And Its Practical Usage](https://www.cyberciti.biz/tips/what-is-devshm-and-its-practical-usage.html#:~:text=%2Fdev%2Fshm%20is%20nothing%20but,speeding%20up%20things%20on%20Linux.)

## PyTorch

### 加速技巧  
1. DataLoader with `num_workers > 0` and `pin_memory = True`
2. `torch.backends.cudnn.benchmark = True` to autotune cudnn kernel choice
3. max out the batch size for each GPU to ammortize compute
4. `bias = False` before BatchNorms
5. use `for p in model.parameters(): p.grad = None` instead of `model.zero_grad()`
6. careful to disable debug APIs in prod(detect_anomaly/profiler/emit_nvtx/gradcheck...)
7. prefer `DistributedDataParallel` to `DataParallel`
8. balance load among GPUs
9. use an apex fused optimizer(default PyTorch optim for loo iterates individual params, yikes)
10. use checkpointing to recompute memory-intensive compute-efficient ops in bwd pass(eg activations, upsampling, ...)
11. use `@torch.jit.script`, e.g. esp to fuse long sequences of pointwise ops like GELU

[PyTorch Performance Tuning Guide](https://www.youtube.com/watch?v=9mS1fIYj1So)


### 分布式训练  
- 分布式训练相关资料  
  - [Distributed model training in PyTorch using DistributedDataParallel](https://spell.ml/blog/pytorch-distributed-data-parallel-XvEaABIAAB8Ars0e)
- 分布式训练的时候， PyTorch 时如何 reduce loss 的， 对于学习率是否需要处理？  
  PyTorch 的分布式训练在每个节点上单独进行forward， loss 和backward 计算， 然后在节点之间传递梯度，做reduce（对梯度取平均）
  所以， 分布式训练的时候， 学习率只要考虑每个节点对应的batch-size下的学习率即可， 不需要根据节点的个数缩放学习率！
  - [Should we split batch_size according to ngpu_per_node when DistributedDataparallel](https://discuss.pytorch.org/t/should-we-split-batch-size-according-to-ngpu-per-node-when-distributeddataparallel/72769)
  - [DistributedDataParallel loss compute and backpropogation?](https://discuss.pytorch.org/t/distributeddataparallel-loss-compute-and-backpropogation/47205)
- **BN 问题**  
  - [How does Dataparallel handels batch norm?](https://discuss.pytorch.org/t/how-does-dataparallel-handels-batch-norm/14040/2)

### VMWare + macOS

- [VMWare Player](https://my.vmware.com/en/web/vmware/downloads/details?downloadGroup=PLAYER-1556&productId=800&rPId=47861#product_downloads)
- [unlocker](https://github.com/paolo-projects/unlocker)
