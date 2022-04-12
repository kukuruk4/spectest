# FIXME Put output not to mcf/src, but to mcf/

cmake_minimum_required(VERSION 3.10)
project(pgdlo)

set(CMAKE_C_COMPILER clang)
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_C_FLAGS "-g")

set(Bench hmmer)

# Use llvm-link to link into LLVM IR
set(CMAKE_C_LINK_EXECUTABLE
        "llvm-link <LINK_FLAGS> <OBJECTS> -o <TARGET> <LINK_LIBRARIES>")

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/..)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/..)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/..)
set(CMAKE_CURRENT_BINARY_DIR  ${CMAKE_CURRENT_BINARY_DIR}/..)

add_executable(${Bench}.bc display.c histogram.c phylip.c sqio.c stopwatch.h emit.c hmmcalibrate.c plan7.c squid.h
                structs.h a2m.c emulation.c hmmio.c plan9.c squidconf.h tophits.c aligneval.c eps.c hmmsearch.c
                postprob.c squidcore.c trace.c alignio.c fast_algorithms.c hsregex.c prior.c sre_ctype.c translate.c
                alphabet.c file.c iupac.c revcomp.c sre_math.c types.c clustal.c funcs.h masks.c rk.c sre_random.c
                ucbqsort.c cluster.c getopt.c mathsupport.c rk.h sre_random.h vectorops.c gki.c misc.c selex.c
                sre_string.c vectorops.h config.h gki.h modelmakers.c seqencode.c ssi.c weight.c core_algorithms.c
                msa.c shuffle.c ssi.h dayhoff.c gsi.c msa.h sqerror.c stack.c debug.c gsi.h msf.c sqfuncs.h
                stockholm.c)

target_compile_options(${Bench}.bc PUBLIC ${CMAKE_C_FLAGS} -emit-llvm)

# Generating LLVM IR (*.ll) file
add_custom_command(TARGET ${Bench}.bc POST_BUILD
        COMMAND llvm-dis ./${Bench}.bc
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating *.ll file..."
        )

# Generating binary file
add_custom_command(TARGET ${Bench}.bc POST_BUILD
        COMMAND clang ${Bench}.bc -o ${Bench}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating binary file..."
        )

#configure_file(RunReorderFields.sh.in
#        ${CMAKE_CURRENT_BINARY_DIR}/RunReorderFields.sh COPYONLY)

#configure_file(../data/train/input/inp.in
#        ${CMAKE_CURRENT_BINARY_DIR}/train.inp.in COPYONLY)
#
#configure_file(../data/ref/input/inp.in
#        ${CMAKE_CURRENT_BINARY_DIR}/ref.inp.in COPYONLY)
#
#configure_file(../data/test/input/inp.in
#        ${CMAKE_CURRENT_BINARY_DIR}/test.inp.in COPYONLY)

#add_dependencies(SynthBench ${SBList})
