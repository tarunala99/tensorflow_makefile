### TensorFlow Makefile

The recommended way to build TensorFlow from source is using the Bazel
open-source build system. Sometimes this isn't possible though:

 - The system may not have the RAM or processing power to support Bazel.
 - Bazel dependencies might not be available.
 - You may want to cross-compile for an unsupported target system.

This experimental project supplies a Makefile automatically derived from the
dependencies listed in the Bazel project, that can be used with GNU's make tool.
It offers the ability to compile the core C++ runtime into a static library, but
doesn't include more advanced features like Python or other language bindings,
or GPU support.

## Building

To compile the library and an example program using it, first pull the
dependencies:

```bash
tensorflow/contrib/makefile/download_dependencies.sh
```

You should only need to do this step once, it puts required libraries like Eigen
in the `tensorflow/contrib/makefile/downloads/` folder. You will also need to
make sure you have a version of [protobuf 3](https://github.com/google/protobuf)
installed on your system, either through package management or building from
source.

Then you can build the project:

```bash
make -f tensorflow/contrib/makefile/Makefile
```

This should compile a static library in 
`tensorflow/contrib/makefile/gen/lib/tf_lib.a`, and create an example executable
at `tensorflow/contrib/makefile/gen/bin/benchmark`. To run the executable, use:

```bash
tensorflow/contrib/makefile/gen/bin/benchmark --graph=tensorflow_inception_graph.pb
```

You should download the example graph from [http://download.tensorflow.org/models/image/imagenet/inception-2015-12-05.tgz](http://download.tensorflow.org/models/image/imagenet/inception-2015-12-05.tgz).

## Supported Systems

The script has been tested on Ubuntu and OS X. If you look in the Makefile
itself, you'll see it's broken up into host and target sections. If you are
cross-compiling, you should look at customizing the target settings to match
what you need for the system you're aiming at.

## Dependencies

The Makefile loads in a list of dependencies stored in text files. These files
are generated from the main Bazel build by running 
`tensorflow/contrib/makefile/gen_file_lists.sh`. You'll need to re-run this i
you make changes to the files that are included in the build.

Header dependencies are not automatically tracked by the Makefile, so if you
make header changes you will need to run this command to recompile cleanly:

```bash
make -f tensorflow/contrib/makefile/Makefile clean
```
