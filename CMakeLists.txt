###############################################################################
#
# File: 		CMakeLists.txt
# Description:	Cmake file to manage and build this latex project. 
# 
###############################################################################
#
# File generated on MMM YYYY by Rin Baudelet
###############################################################################
cmake_minimum_required(VERSION 3.19 FATAL_ERROR)
project(latex_thesis_template)

# Setup LaTeX
set(LATEX_COMPILER_FLAGS "-interaction=batchmode -file-line-error -shell-escape"
	CACHE STRING "Flags passed to latex.")
set(LATEX_USE_SYNCTEX 1)

set(imgdir              img)
set(srcdir              src)
set(stydir              sty)
set(svgdir				svg)
set(pdfdir				pdf)
set(figdir				fig)

# UseLATEX
if (NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/UseLATEX.cmake")
	message(STATUS "UseLATEX.cmake missing... Setting up this project!")

	message(STATUS "Download UseLATEX.cmake...")
	file(DOWNLOAD "https://gitlab.kitware.com/kmorel/UseLATEX/-/raw/main/UseLATEX.cmake" "${CMAKE_CURRENT_SOURCE_DIR}/UseLATEX.cmake")

	message(STATUS "Create project's architecture...")

	message(STATUS "Make directory for contents")
	file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${srcdir}/chp)

	message(STATUS "Make directory for figures")
	file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${srcdir}/${figdir})

	message(STATUS "Make directory for pdf")
	file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${pdfdir})

	message(STATUS "Make directory for images")
	file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${imgdir})

	message(STATUS "Make directory for SVG")
	file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${svgdir})

	message(STATUS "Make directory for STY")
	file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${stydir})

	message(STATUS "Project successfuly set up !")
endif()
include(UseLATEX.cmake)

# Global
set(LATEX_USE_SYNCTEX   ON)

file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/pdf)
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/buidfig)
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/figures)

# Files
file(GLOB_RECURSE src RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${srcdir}/*.tex)
file(GLOB_RECURSE bib RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${srcdir}/*.bib)
file(GLOB_RECURSE sty RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${stydir}/*.sty)
file(GLOB_RECURSE svg RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${svgdir}/*.SVG)
file(GLOB_RECURSE pdf RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${pdfdir}/*.pdf)

set(latex_document_options)
set(custom_command_options ALL)

add_latex_document( main.tex
        INPUTS ${src} ${lst} ${sty} ${svg} ${pdf}
        BIBFILES ${bib}
        IMAGE_DIRS ${imgdir}

        USE_BIBLATEX
        #USE_GLOSSARY
		#USE_INDEX
		#INDEX_NAMES general functions
        ${latex_document_options}
		FORCE_PDF
)

add_custom_command(TARGET main POST_BUILD
		COMMAND ${CMAKE_COMMAND} -E copy_if_different
		${CMAKE_CURRENT_BINARY_DIR}/*.pdf ${CMAKE_CURRENT_BINARY_DIR}/pdf
		)