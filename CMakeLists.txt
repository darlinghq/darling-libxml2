project(libxml2)

cmake_minimum_required(VERSION 3.10)

enable_language(C ASM)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_definitions(
	-DHAVE_CONFIG_H
	-D_REENTRANT
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib")

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/libxml2/include
	${CMAKE_CURRENT_SOURCE_DIR}/libxml2
	"${CMAKE_CURRENT_SOURCE_DIR}/Pregenerated Files/include"
	"${CMAKE_CURRENT_SOURCE_DIR}/Pregenerated Files"
)

set(xml2_SRCS
	libxml2/buf.c
	libxml2/c14n.c
	libxml2/catalog.c
	libxml2/chvalid.c
	libxml2/debugXML.c
	libxml2/dict.c
	libxml2/DOCBparser.c
	libxml2/encoding.c
	libxml2/entities.c
	libxml2/error.c
	libxml2/globals.c
	libxml2/hash.c
	libxml2/HTMLparser.c
	libxml2/HTMLtree.c
	libxml2/legacy.c
	libxml2/list.c
	libxml2/nanoftp.c
	libxml2/nanohttp.c
	libxml2/parser.c
	libxml2/parserInternals.c
	libxml2/pattern.c
	libxml2/relaxng.c
	libxml2/SAX.c
	libxml2/SAX2.c
	libxml2/schematron.c
	libxml2/threads.c
	libxml2/tree.c
	libxml2/uri.c
	libxml2/valid.c
	libxml2/xinclude.c
	libxml2/xlink.c
	libxml2/xmlIO.c
	libxml2/xmlmemory.c
	libxml2/xmlmodule.c
	libxml2/xmlreader.c
	libxml2/xmlregexp.c
	libxml2/xmlsave.c
	libxml2/xmlschemas.c
	libxml2/xmlschemastypes.c
	libxml2/xmlstring.c
	libxml2/xmlunicode.c
	libxml2/xmlwriter.c
	libxml2/xpath.c
	libxml2/xpointer.c
	libxml2/xzlib.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/libxml2.2.dylib")
set(DYLIB_COMPAT_VERSION "10.0.0")
set(DYLIB_CURRENT_VERSION "10.9.0")
add_darling_library(xml2 SHARED ${xml2_SRCS})
target_link_libraries(xml2 system icucore z)
set_target_properties(xml2 PROPERTIES OUTPUT_NAME "xml2.2")
target_link_options(xml2 PRIVATE
	"-Wl,-exported_symbols_list,${CMAKE_CURRENT_SOURCE_DIR}/Pregenerated Files/libxml2.exp"
)
make_fat(xml2)

install(TARGETS xml2 DESTINATION libexec/darling/usr/lib)
InstallSymlink("libxml2.2.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libxml2.dylib")

install(FILES libxml2/xml2-config.1 DESTINATION libexec/darling/usr/share/man/man1)
install(FILES libxml2/libxml.3 DESTINATION libexec/darling/usr/share/man/man3)
