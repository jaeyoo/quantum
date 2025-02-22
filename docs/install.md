# Install TensorFlow Quantum

There are a few ways to set up your environment to use TensorFlow Quantum (TFQ):

* The easiest way to learn and use TFQ requires no installation—run the
  [TensorFlow Quantum tutorials](./tutorials/hello_many_worlds.ipynb) directly
  in your browser using
  [Google Colab](https://colab.research.google.com/github/tensorflow/quantum/blob/master/docs/tutorials/hello_many_worlds.ipynb).
* To use TensorFlow Quantum on a local machine, install the TFQ package using
  Python's pip package manager.
* Or build TensorFlow Quantum from source.

TensorFlow Quantum is supported on Python 3.7, 3.8, and 3.9 and depends directly on [Cirq](https://github.com/quantumlib/Cirq).

## Pip package

### Requirements

* pip 23.0 or later (requires `manylinux2014` support)
* [TensorFlow == 2.11.0](https://www.tensorflow.org/install/pip)

See the [TensorFlow install guide](https://www.tensorflow.org/install/pip) to
set up your Python development environment and an (optional) virtual environment.

Upgrade `pip` and install TensorFlow

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">pip3 install --upgrade pip</code>
  <code class="devsite-terminal">pip3 install tensorflow==2.11.0</code>
</pre>
<!-- common_typos_enable -->

### Install the package

Install the latest stable release of TensorFlow Quantum:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">pip3 install -U tensorflow-quantum</code>
</pre>
<!-- common_typos_enable -->

Success: TensorFlow Quantum is now installed.

Nightly builds which might depend on newer version of TensorFlow can be installed with:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">pip3 install -U tfq-nightly</code>
</pre>
<!-- common_typos_enable -->

## Build from source

The following steps are tested for Ubuntu-like systems.

### 1. Set up a Python 3 development environment

First we need the Python 3.8 development tools.
<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">sudo apt update</code>
  <code class="devsite-terminal">sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python3.8</code>
  <code class="devsite-terminal">sudo apt install python3.8 python3.8-dev python3.8-venv python3-pip</code>
  <code class="devsite-terminal">python3.8 -m pip install --upgrade pip</code>
</pre>
<!-- common_typos_enable -->

### 2. Create a virtual environment

Go to your workspace directory and make a virtual environment for TFQ development.
<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">python3.8 -m venv quantum_env</code>
  <code class="devsite-terminal">source quantum_env/bin/activate</code>
</pre>
<!-- common_typos_enable -->

### 3. Install Bazel

As noted in the TensorFlow
[build from source](https://www.tensorflow.org/install/source#install_bazel)
guide, the <a href="https://bazel.build/" class="external">Bazel</a>
build system will be required.

Our latest source builds use TensorFlow 2.11.0. To ensure compatibility we use `bazel` version 5.3.0. To remove any existing version of Bazel:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">sudo apt-get remove bazel</code>
</pre>
<!-- common_typos_enable -->

Download and install `bazel` version 5.3.0:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">wget https://github.com/bazelbuild/bazel/releases/download/5.3.0/bazel_5.3.0-linux-x86_64.deb
</code>
  <code class="devsite-terminal">sudo dpkg -i bazel_5.3.0-linux-x86_64.deb</code>
</pre>
<!-- common_typos_enable -->

To prevent automatic updating of `bazel` to an incompatible version, run the following:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">sudo apt-mark hold bazel</code>
</pre>
<!-- common_typos_enable -->

Finally, confirm installation of the correct `bazel` version:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">bazel --version</code>
</pre>
<!-- common_typos_enable -->


### 4. Build TensorFlow from source

Here we adapt instructions from the TensorFlow [build from source](https://www.tensorflow.org/install/source)
guide, see the link for further details. TensorFlow Quantum is compatible with TensorFlow version&nbsp;2.11.0.

Download the
<a href="https://github.com/tensorflow/tensorflow" class="external">TensorFlow source code</a>:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">git clone https://github.com/tensorflow/tensorflow.git</code>
  <code class="devsite-terminal">cd tensorflow</code>
  <code class="devsite-terminal">git checkout v2.11.0</code>
</pre>

Be sure the virtual environment you created in step 2 is activated. Then, install the TensorFlow dependencies:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">pip install -U pip six numpy wheel setuptools mock 'future>=0.17.1'</code>
  <code class="devsite-terminal">pip install -U keras_applications --no-deps</code>
  <code class="devsite-terminal">pip install -U keras_preprocessing --no-deps</code>
  <code class="devsite-terminal">pip install numpy==1.24.2</code>
  <code class="devsite-terminal">pip install packaging requests</code>
</pre>
<!-- common_typos_enable -->

Configure the TensorFlow build. When asked for the Python interpreter and library locations, be sure to specify locations inside your virtual environment folder.  The remaining options can be left at default values.

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">./configure</code>
</pre>
<!-- common_typos_enable -->

Build the TensorFlow package (Since TF v2.8, `_GLIBCXX_USE_CXX11_ABI` is set to 1, and the c++ codes are all compiled with `-std=c++17`):

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">bazel build -c opt --cxxopt="-O3" --cxxopt="-march=native" --cxxopt="-std=c++17" --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=1" //tensorflow/tools/pip_package:build_pip_package</code>
</pre>
<!-- common_typos_enable -->

Note: It may take over an hour to build the package.

After the build is complete, install the package and leave the TensorFlow directory:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg</code>
  <code class="devsite-terminal">pip install /tmp/tensorflow_pkg/<var>name_of_generated_wheel</var>.whl</code>
  <code class="devsite-terminal">cd ..</code>
</pre>
<!-- common_typos_enable -->

### 5. Download TensorFlow Quantum

We use the standard [fork and pull request workflow](https://guides.github.com/activities/forking/) for contributions.  After forking from the [TensorFlow Quantum](https://github.com/tensorflow/quantum) GitHub page, download the source of your fork and install the requirements:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">git clone https://github.com/<var>username</var>/quantum.git</code>
  <code class="devsite-terminal">cd quantum</code>
  <code class="devsite-terminal">pip install -r requirements.txt</code>
</pre>
<!-- common_typos_enable -->


### 6. Build the TensorFlow Quantum pip package for CPU

Build the TensorFlow Quantum pip package and install:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">./configure.sh  # Type 'Y' for the first question.</code>
  <code class="devsite-terminal">bazel build -c opt --cxxopt="-O3" --cxxopt="-march=native" --cxxopt="-std=c++17" --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=1" release:build_pip_package</code>
  <code class="devsite-terminal">bazel-bin/release/build_pip_package /tmp/tfquantum/</code>
  <code class="devsite-terminal">python3 -m pip install /tmp/tfquantum/<var>name_of_generated_wheel</var>.whl</code>
</pre>
<!-- common_typos_enable -->

To confirm that TensorFlow Quantum for CPU has successfully been installed, you can run the tests:
<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">./scripts/test_all.sh</code>
</pre>
<!-- common_typos_enable -->


Success: TensorFlow Quantum for CPU is now installed.

### 7. Build the TensorFlow Quantum pip package for GPU

To enable GPU (cuQuantum) backend, cuStatevec must be installed, see <a href="https://docs.nvidia.com/cuda/cuquantum/custatevec/getting_started.html">installation guide</a> for details. Importantly, we require that the `CUQUANTUM_ROOT` environment variable has been set by running the following with your installation path.
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">export CUQUANTUM_ROOT=/path/to/cuquantum/installation/dir </code>
</pre>

Build the TensorFlow Quantum GPU pip package and install:

<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">bazel clean --expunge  # If you got stuck `.so` related issue, please clean the cache.</code>
  <code class="devsite-terminal">./configure.sh  # Type 'n' for the second question.</code>
  <code class="devsite-terminal">bazel build -c opt --config=cuda --cxxopt="-O3" --cxxopt="-march=native" --cxxopt="-std=c++17" --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=1" release:build_pip_package</code>
  <code class="devsite-terminal">bazel-bin/release/build_pip_package /tmp/tfquantum_gpu/</code>
  <code class="devsite-terminal">python3 -m pip install /tmp/tfquantum_gpu/<var>name_of_generated_wheel</var>.whl</code>
</pre>
<!-- common_typos_enable -->

To confirm that TensorFlow Quantum for GPU has successfully been installed, you can run the tests:
<!-- common_typos_disable -->
<pre class="devsite-click-to-copy">
  <code class="devsite-terminal">./scripts/test_all.sh gpu</code>
</pre>
<!-- common_typos_enable -->


Success: TensorFlow Quantum for GPU is now installed.
