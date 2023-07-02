### 安装CUDA和cuDNN
1. 查看CUDA版本：```win + R``` 打开cmd命令行窗口，输入 ```nvidia-smi``` 后，可在 ```Driver Verison``` 里查看NVIDIA驱动版本，在 ```CUDA Version``` 里查看CUDA版本
	```
	# 自己的Driver Verison和CUDA Version记录（20230701）
	Driver Version: 531.41
	CUDA Version: 12.1
	```
2. 下载CUDA和cuDNN
	* CUDA
		
		查看CUDA版本后，进入CUDA Toolkit Archive：https://developer.nvidia.com/cuda-toolkit-archive，在Archived Releases中找到自己显卡CUDA版本对应的CUDA Toolkit。**下载版本不能高于自己显卡的版本**，比如此处我只能选择CUDA Toolkit 12.1.0版本
	* cuDNN
		
		下载cuDNN需要有NVIDIA的账号。拥有账号后，在https://developer.nvidia.com/rdp/cudnn-archive选择匹配自己CUDA版本的cuDNN进行下载
		```
		# 自己的cuDNN版本记录（20230701）
		cuDNN v8.9.0(April 11th, 2023) for CUDA 12.X
		```
3. CUDA安装
	1. 双击.exe文件
	2. 对话框中出现的目录为临时解压目录，完成后会自动删除，因此可直接进行下一步
	3. 许可协议：同意并继续
	4. 选项：自定义并全选，下一步
	5. 安装位置：自定义安装位置（CUDA Development和CUDA Documentation可安装在同一目录下），下一步。若未安装CUDA Visual Studio，会在点击下一步后提示安装，同意即可
		```
		# 自己的安装位置记录
		C:/User/Software/CUDA12.1
		```
	6. 等待安装结束
	7. 检查CUDA是否成功安装：```win + R``` 打开cmd命令行窗口，输入 ```nvcc -V```
		```
		# 自己的CUDA release版本记录
		Cuda compilation tools, release 12.1 V12.1.66
		```
4. cuDNN安装（替换文件）
	1. 解压缩下载的压缩包文件
	2. 将 ```bin```、```include```、```lib``` 三个文件夹复制到CUDA安装的根目录下（即上述的CUDA12.1文件夹）即可
### 安装Pytorch
自己直接使用Anaconda进行安装的Pytorch是CPU版本，因此自己想办法安装了支持GPU的版本
1. Conda环境下安装
	1. 进入pytorch下载界面：https://pytorch.org/get-started/previous-versions/，挑选适配自己CUDA的pytorch版本
	2. 通过Anaconda Powershell Prompt或在cmd命令行窗口中激活要安装pytorch的Conda虚拟环境
	3. 输入对应的命令，回车安装
		```
		# 自己安装输入的内容记录
		conda install pytorch==2.0.0 torchvision==0.15.0 torchaudio==2.0.0 pytorch-cuda=11.8 -c pytorch -c nvidia
		```
2. pip安装
	1. 命令行如下
		```
		pip install torch==2.0.0+cu118 torchvision==0.15.1+cu118 torchaudio==2.0.1 --index-url https://download.pytorch.org/whl/cu118
		```
3. 下载离线包安装
	1. 下载
		
		介于Conda和pip在线安装的速度，推荐下载离线包安装：https://download.pytorch.org/whl/torch_stable.html。下载文件包括三个：torch、torchaudio和torchvision
		```
		# 文件名示例
		# cu开头代表GPU（CUDA）版本，如cu118代表CUDA11.8
		# cp开头代表python版本，如cp310代表python3.10.x
		# win_amd64代表适配的操作系统版本
		# %2B代表beta版本
	
		# 自己下载的内容记录
		cu118/torch-2.0.0%2Bcu118-cp310-cp310-win_amd64.whl
		torchaudio-2.0.0+cu118-cp310-cp310-win_amd64.whl
		torchvision-0.15.0+cu118-cp310-cp310-win_amd64.whl
		```
	2. 安装
		1. 通过Anaconda Powershell Prompt或在cmd命令行窗口中激活要安装pytorch的Conda虚拟环境
		2. 通过 ```cd``` 命令进入下载该三个文件的目录中
		3. 依次使用 ```pip install``` 安装torch、torchvision、torchaudio文件。安装时要输入完整的名称
		4. 进入python解释器，输入以下代码得到示例输出结果即为安装成功
			```python
			import torch
			print(torch.__version__)
			>>> 2.0.0+cu118  # 安装了CUDA版本
			print(torch.cuda.is_available())
			>>> True  # CUDA激活
			print(torch.cuda.device_count())
			>>> 1  # 查看GPU数量
			print(torch.version.cuda)
			>>> 11.8  # 查看CUDA版本
			```
		

