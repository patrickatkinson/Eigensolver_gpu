# Flags for GPUs with Volta architecture. Change cc70 to cc60 if compiling for Pascal.
GPU_ARCH=70

FLAGS = -fpic -O3 -mp -pgf90libs -Mcuda=cc$(GPU_ARCH),cuda11.0,ptxinfo -Mlarge_arrays
FLAGS2 = -fpic -O3 -mp -pgf90libs -Mcuda=cc$(GPU_ARCH),cuda11.0,ptxinfo,maxregcount:64 -Mlarge_arrays

# For performance reasons, some files are compiled with cc60, even on Volta. Do not update the cc value in FLAGS3! 
FLAGS3 = -fpic -O3 -mp -pgf90libs -Mcuda=cc60,cuda11.0,ptxinfo,nordc,maxregcount:255  -Mlarge_arrays

# Uncomment to enable NVTX markers
#OPTFLAGS = -DUSE_NVTX

all: lib_eigsolve.a lib_eigsolve.so

OBJS = eigsolve_vars.o toolbox.o zhegst_gpu.o zhemv_gpu.o zhetd2_gpu.o zhetrd_gpu.o zheevd_gpu.o zhegvdx_gpu.o \
       dsygst_gpu.o dsymv_gpu.o dsytd2_gpu.o dsytrd_gpu.o dsyevd_gpu.o dsygvdx_gpu.o

zhetd2_gpu.o : zhetd2_gpu.F90
	pgf90 -c ${FLAGS2} ${OPTFLAGS}  $*.F90 -o $*.o
zhemv_gpu.o : zhemv_gpu.F90
	pgf90 -c ${FLAGS3} ${OPTFLAGS}  $*.F90 -o $*.o
dsytd2_gpu.o : dsytd2_gpu.F90
	pgf90 -c ${FLAGS2} ${OPTFLAGS}  $*.F90 -o $*.o
dsymv_gpu.o : dsymv_gpu.F90
	pgf90 -c ${FLAGS3} ${OPTFLAGS}  $*.F90 -o $*.o
%.o: %.cuf
	pgf90 -c ${FLAGS} ${OPTFLAGS} $*.cuf -o $*.o
%.o: %.F90
	pgf90 -c ${FLAGS} ${OPTFLAGS} $*.F90 -o $*.o

lib_eigsolve.a: $(OBJS)
	ar -cr lib_eigsolve.a $(OBJS)

lib_eigsolve.so: $(OBJS)
	pgf90 -shared ${FLAGS} -o $@ $(OBJS)

clean:
	rm -f lib_eigsolve.a *.mod *.o
