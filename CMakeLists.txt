cmake_minimum_required (VERSION 3.11)
project(polybench)

include_directories(${CMAKE_CURRENT_LIST_DIR}/utilities)

# Define the polybench data type here
# We want to use 32-bit floats to work well with wasm
add_definitions(-DDATA_TYPE_IS_FLOAT=1)

function(polybench_func exec_name dir_path)
    add_executable(poly_${exec_name}
            ${dir_path}/${exec_name}/${exec_name}.c
            ${dir_path}/${exec_name}/${exec_name}.h
            utilities/polybench.c
            utilities/polybench.h
            )

    if (FAASM_BUILD_TYPE STREQUAL "wasm")
        target_link_libraries(poly_${exec_name} faasm)
        target_compile_definitions(poly_${exec_name} PUBLIC __wasm__ __faasm)
    else()
        target_link_libraries(poly_${exec_name} m)
    endif()

    # Add this func to the list of all functions
    set(ALL_POLY_FUNCS ${ALL_POLY_FUNCS} poly_${exec_name} PARENT_SCOPE)

endfunction(polybench_func)

polybench_func(covariance datamining)
polybench_func(correlation datamining)

polybench_func(2mm linear-algebra/kernels)
polybench_func(3mm linear-algebra/kernels)
polybench_func(atax linear-algebra/kernels)
polybench_func(bicg linear-algebra/kernels)
polybench_func(doitgen linear-algebra/kernels)
polybench_func(mvt linear-algebra/kernels)

polybench_func(cholesky linear-algebra/solvers)
polybench_func(durbin linear-algebra/solvers)
polybench_func(gramschmidt linear-algebra/solvers)
polybench_func(lu linear-algebra/solvers)
polybench_func(ludcmp linear-algebra/solvers)
polybench_func(trisolv linear-algebra/solvers)

polybench_func(deriche medley)
polybench_func(floyd-warshall medley)
polybench_func(nussinov medley)

polybench_func(adi stencils)
polybench_func(fdtd-2d stencils)
polybench_func(heat-3d stencils)
polybench_func(jacobi-1d stencils)
polybench_func(jacobi-2d stencils)
polybench_func(seidel-2d stencils)

# Custom target to group all the poly functions
add_custom_target(polybench_all DEPENDS ${ALL_POLY_FUNCS})

