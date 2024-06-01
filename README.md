
# Research and exploration of this paper for the purpose of understanding and the implementation of security of DNN models deployed on edge devices.
# Finding ways to make the implementation better and finding the loop holes.
# Testing this approach of this paper against other papers to test the potential of the security approach.

# ModelObfuscator

This is protptype tool of paper: **Modelobfuscator: Obfuscating Model Information to Protect Deployed ML-Based Systems on ISSTA2023**.

**Abstract**:
More and more edge devices and mobile apps are leveraging deep learning (DL) capabilities. Deploying such models on devices – referred to as on-device models – rather than as remote cloud-hosted services, has gained popularity because it avoids transmitting user’s data off of the device and achieves high response time. However, on-device models can be easily attacked, as they can be accessed by unpacking corresponding apps and the model is fully exposed to attackers. Recent studies show that attackers can easily generate white-box-like attacks for an on-device model or even inverse its training data. To protect on-device models from white-box attacks, we propose a novel technique called model obfuscation. Specifically, model obfuscation hides and obfuscates the key information – structure, parameters and attributes – of models by renaming, parameter encapsulation, neural structure obfuscation obfuscation, shortcut injection, and extra layer injection. We have developed a prototype tool ModelObfuscator to automatically obfuscate on-device TFLite models. Our experiments show that this proposed approach can dramatically improve model security by significantly increasing the difficulty of parsing models’ inner information, without in- creasing the latency of DL models. Our proposed on-device model obfuscation has the potential to be a fundamental technique for on-device model deployment

Wo provide two options to use our prototype tool:

## 1*. Preparation A: run by Docker (recommend)

(0) Download the Docker Image:

```
docker pull zhoumingyigege/modelobfuscator:latest
```

Note that if it cause permission errors, please try: 

```
sudo docker pull zhoumingyigege/modelobfuscator:latest
```

(1) Enter the environment:

```
docker run -i -t zhoumingyigege/modelobfuscator:latest /bin/bash
```

Note that if it cause permission errors, please try: 

```
sudo docker run -i -t zhoumingyigege/modelobfuscator:latest /bin/bash
```

Enter the project:

```
cd Code275/
```

(2) Activate the conda environment: 

```
conda activate code275
```

## 1*. Preparation B: build the environment

(0) Download the code:

```
git clone https://github.com/zhoumingyi/ModelObfuscator.git
cd ModelObfuscator
```

(1) The dependency can be found in `environment.yml`. To create the conda environment:

```
conda env create -f environment.yml
conda activate code275
```

Install the Flatbuffer:

```
conda install -c conda-forge flatbuffers
```

(if no npm) install the npm:

```
sudo apt-get install npm
```

Install the jsonrepair:

```
npm install -g jsonrepair
```

Note that the recommend version of gcc and g++ is 9.4.0.


(2) Download the source code of the TensorFlow. Here we test our tool on v2.9.1.

```
wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.9.1.zip
```

Unzip the file:

```
unzip v2.9.1
```

(3) Download the Bazel:

```
wget https://github.com/bazelbuild/bazelisk/releases/download/v1.14.0/bazelisk-linux-amd64
chmod +x bazelisk-linux-amd64
sudo mv bazelisk-linux-amd64 /usr/local/bin/bazel
```

You can test the Bazel:

```
which bazel
```

It should return:

```
# in ubuntu
/usr/local/bin/bazel
```

(4) Configure the build:

```
cd tensorflow-2.9.1/
./configure
cd ..
```

You can use the default setting (just type Return/Enter for every option).

(5) Copy the configurations and script to the source code:  

```
cp ./files/kernel_files/* ./tensorflow-2.9.1/tensorflow/lite/kernels/
cp ./files/build_files/build.sh ./tensorflow-2.9.1/
```

Note that you can mofify the maximal number of jobs in the 'build.sh' script. Here I set it as `--jobs=14`. 

## 2. Test

(1) Build the obfuscation model:

```
bash build_obf.sh
```

Note that you can modify the test model and obfuscation parameters in the script. The obfuscated model is saved as the 'obf_model.tflite'.
