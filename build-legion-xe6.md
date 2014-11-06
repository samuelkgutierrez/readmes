Building and Running Legion Applications on a Cray XE6
======================================================

Last Updated: Thu Nov  6 11:50:56 MST 2014

## Building Legion for the Cray XE6

- Swap out current programming environment

```bash
module swap  PrgEnv-pgi  PrgEnv-gnu
```

- Build [GASNet](http://gasnet.lbl.gov/).

```bash
# Setup install prefix environment variable for future use.
export GASNET_HOME=/gasnet/install/path

# Untar distribution and cd into the created distribution directory.
tar xzf GASNet-1.24.0.tar.gz && cd GASNet-1.24.0

# Configure (NOTE: use --enable-cross-compile if necessary)
./configure --prefix=$GASNET_HOME \
--enable-gemini \
GNI_INCLUDES="-I/opt/cray/gni-headers/default/include -I/opt/cray/pmi/default/include" \
GNI_LIBS="-L/opt/cray/ugni/default/lib64/ -lugni -L/opt/cray/pmi/default/lib64 -lpmi" \
--disable-pshm --disable-mpi --disable-ibv --disable-udp --enable-par \
--enable-segment-fast --disable-aligned-segments --disable-pshm \
--with-segment-mmap-max=4GB

# Build and install it
make -j4 && make install
```

- Build your [Legion](http://legion.stanford.edu/) Application

This requires (at the moment) that you modify your application's Makefile.

- Change **SHARED_LOWLEVEL=1** to **SHARED_LOWLEVEL=0**. This tells Legion to
  use the GASNet version.

- Add **GASNET_ROOT := [value of $GASNET_HOME]**. This specifies the
  installation prefix of GASNe to the Legion build system.

- Add **CONDUIT := gemini** to the Makefile. This is how one specifies which
  GASNet conduit will be used for this particular build.

- Change **OUTPUT_LEVEL=LEVEL_DEBUG** to **OUTPUT_LEVEL=LEVEL_ERROR** to silence
  lots of Legion debugging output.

- Add **USE_CUDA := 0**, since the XE6 does not have GPUs.

- Modify **LD_FLAGS** to include some linker flags that are required for PMI and
  uGNI For example:

```
LD_FLAGS := \
-L/opt/cray/pmi/default/lib64 -lpmi \
-L/opt/cray/ugni/default/lib64 -lugni
```

```bash
export LG_RT_DIR=/path/to/legion/runtime
make -j4
```
At this point, you should have a Legion executable.

## Running Your Application on the Cray XE6

- Get an allocation. For example:

```bash
msub -I -l nodes=2:ppn=16,walltime=01:00:00
```

- Run. For example:

```bash
# Notice that we are running 1 process per node and turning off process
# affinity.
aprun -cc none -n 2 -N ./my-legion-app
```
