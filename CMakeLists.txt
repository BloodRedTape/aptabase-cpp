cmake_minimum_required (VERSION 3.8)

project ("Aptabase-Cpp")

add_library(AptabaseCpp 
	"./sources/model.cpp"
	"./sources/provider.cpp"
)
target_link_libraries(AptabaseCpp 
	PUBLIC nlohmann_json 
)

if(CMAKE_APTABASE_USE_HTTPLIB)
	target_link_libraries(AptabaseCpp PUBLIC httplib)
	target_compile_definitions(AptabaseCpp PUBLIC APTABASE_USE_HTTPLIB=1)
endif()

if(CMAKE_APTABASE_USE_BOOST)
	find_package(Boost REQUIRED)
	find_package(OpenSSL REQUIRED)

	target_link_directories(AptabaseCpp PUBLIC ${Boost_LIBRARY_DIR_RELEASE})
	target_include_directories(AptabaseCpp PUBLIC ${OPENSSL_INCLUDE_DIR} PUBLIC ${Boost_INCLUDE_DIR})
	target_link_libraries(AptabaseCpp PUBLIC ${OPENSSL_LIBRARIES} PUBLIC ${Boost_LIBRARIES})

	target_compile_definitions(AptabaseCpp PUBLIC APTABASE_USE_BOOST=1)
endif()

target_include_directories(AptabaseCpp 
	PUBLIC "./include"
)
target_precompile_headers(AptabaseCpp 
	PUBLIC "./include/aptabase/model.hpp"
)
target_compile_features(AptabaseCpp PRIVATE cxx_std_17)