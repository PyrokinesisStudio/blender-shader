
set(INC
	.
	../graph
	../kernel
	../kernel/svm
	../kernel/osl
	../util
	../render
	../../glew-mx
)

set(INC_SYS
	${GLEW_INCLUDE_DIR}
	../../../extern/clew/include
)

if(WITH_CUDA_DYNLOAD)
	list(APPEND INC
		../../../extern/cuew/include
	)
	add_definitions(-DWITH_CUDA_DYNLOAD)
else()
	list(APPEND INC_SYS
		${CUDA_TOOLKIT_INCLUDE}
	)
	add_definitions(-DCYCLES_CUDA_NVCC_EXECUTABLE="${CUDA_NVCC_EXECUTABLE}")
endif()

set(SRC
	device.cpp
	device_cpu.cpp
	device_cuda.cpp
	device_multi.cpp
	device_opencl.cpp
	device_task.cpp
)

if(WITH_CYCLES_NETWORK)
	list(APPEND SRC
		device_network.cpp
	)
endif()

set(SRC_HEADERS
	device.h
	device_memory.h
	device_intern.h
	device_network.h
	device_task.h
)

add_definitions(${GL_DEFINITIONS})
if(WITH_CYCLES_NETWORK)
	add_definitions(-DWITH_NETWORK)
endif()
if(WITH_CYCLES_DEVICE_OPENCL)
	add_definitions(-DWITH_OPENCL)
endif()
if(WITH_CYCLES_DEVICE_CUDA)
	add_definitions(-DWITH_CUDA)
endif()
if(WITH_CYCLES_DEVICE_MULTI)
	add_definitions(-DWITH_MULTI)
endif()

include_directories(${INC})
include_directories(SYSTEM ${INC_SYS})

add_library(cycles_device ${SRC} ${SRC_HEADERS})
