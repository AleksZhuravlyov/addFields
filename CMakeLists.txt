cmake_minimum_required(VERSION 3.8)

project(addFields)

set(CMAKE_CXX_STANDARD 14)


include_directories(.)
include_directories($ENV{FOAM_SRC}/OpenFOAM/lnInclude)
include_directories($ENV{FOAM_SRC}/OSspecific/POSIX/lnInclude)
include_directories($ENV{FOAM_SRC}/genericPatchFields/lnInclude)
include_directories($ENV{FOAM_SRC}/finiteVolume/lnInclude)
include_directories($ENV{FOAM_SRC}/meshTools/lnInclude)

link_directories($ENV{FOAM_LIBBIN})

if (APPLE AND EXISTS /opt/homebrew/lib)
    link_directories(/opt/homebrew/lib)
endif ()


add_definitions(
        -D$ENV{WM_ARCH}
        -DWM_ARCH_OPTION=$ENV{WM_ARCH_OPTION}
        -DWM_$ENV{WM_PRECISION_OPTION}
        -DWM_LABEL_SIZE=$ENV{WM_LABEL_SIZE}
	    -DNoRepository -m64 -fPIC)


add_executable(${PROJECT_NAME} ${PROJECT_NAME}.C)

set(CMAKE_EXE_LINKER_FLAGS -Wl,-execute,-undefined,dynamic_lookup,-w)

target_link_libraries(${PROJECT_NAME}
        OpenFOAM        
        finiteVolume       
        fvModels
        fvConstraints
		genericPatchFields
        meshTools        
        dl
        m)
