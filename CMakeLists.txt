cmake_minimum_required(VERSION 3.5)
project(digest)
aux_source_directory(src SOURCE)
add_library(digest ${SOURCE})