# anaconda安装pytorch的虚拟环境

  步骤如下：

```shell
1 sh Anaconda3-2020.11-Linux-x86_64.sh

2 conda list 

3 python --version

4 conda install pytorch torchvision torchaudio cpuonly -c pytorch

5 conda list pytorch

6 conda create -n torch python=3.8.5

7 conda info --envs

8 conda activate torch
```

