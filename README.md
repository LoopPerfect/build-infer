# build-infer

Self-service distributions of BuildInfer. 

We usually like to be on-hand to guide the transpilation process and fix any issues that arise. If that does not work for your use-case, then we also provide this self-service page. 

If you run into any problems, please get in-touch (support@loopperfect.com) or [create an issue](https://github.com/LoopPerfect/build-infer/issues/new). 

## Usage

Make sure that you have `strace` installed on your system. For Debian, this is: 

```bash=
sudo apt-get update
sudo apt-get install -y strace
```

Fetch the latest release from the [releases page](https://github.com/LoopPerfect/build-infer/releases): 

```bash=
wget -O buildinfer https://github.com/LoopPerfect/build-infer/releases/download/v1.0.0/buildinfer
chmod +x ./buildinfer
```

Create an alias so that you can call BuildInfer from anywhere: 

```bash=
alias buildinfer=`pwd`/buildinfer
```

Test that it works: 

```bash=
$ buildinfer
BuildInfer - LoopPerfect
https://buildinfer.loopperfect.com/

Usage: 
1. Navigate to your build folder
2. Run `buildinfer record <build-command>`
     e.g. `buildinfer record make -j4`
3. cd to your project-root
4. Run `buildinfer analyze <project-root> <build-root>`
```

Now, `cd` to your C/C++ project. 

Record the build: 

```bash=
mkdir build
cd ./build
cmake ..
buildinfer record make
```

Analyze the trace: 

```bash=
cd ../
buildinfer analyze . ./build
```

Generate the new build script: 

```bash=
buildinfer transpile > BUCK
```

If you do not already have [Buck](https://buckbuild.com/) installed, we recommend using the [Debian package](https://github.com/facebook/buck/releases/tag/v2018.10.29.01). 

View the build script: 

```bash=
cat ./BUCK
```

View the targets: 

```bash=
touch .buckconfig
buck targets //...
```

## Graphing

BuildInfer provides some functionality to generate a `*.dot` file that can be used to render using [GraphViz](https://www.graphviz.org/)

- `buildinfer graph > graph.dot`

     renders whole dependency graph

- `buildinfer graph-minimal > graph-minimal.dot`

     renders interaction graph between applications and files


You can then use for example `sfdp` from the grapviz package to render an svg:

`sfdp -Tsvg -Goutputmode=edgesfirst -Goverlap=prism graph.dot > graph.svg`
