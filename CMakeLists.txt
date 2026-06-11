
project(implot)


include(FetchContent)

FetchContent_Declare(imgui
    GIT_REPOSITORY
        https://github.com/bostick/imgui.git
    GIT_TAG
        milestone-7-branch
    GIT_SHALLOW 1
    GIT_PROGRESS 1
)
FetchContent_MakeAvailable(imgui)


add_library(implot-lib STATIC
    implot.cpp
    implot_items.cpp
)

target_include_directories(implot-lib
    PUBLIC
        .
)

target_link_libraries(implot-lib
    PRIVATE
        imgui-lib
)

#
# Set up warnings
#
# https://www.foonathan.net/2018/10/cmake-warnings/
#
target_compile_options(implot-lib
    PRIVATE
        -Wall -Wextra -pedantic -Werror -Wconversion -Wno-sign-conversion -Wno-implicit-int-float-conversion
)

target_compile_definitions(implot-lib
    PRIVATE
        "IMPLOT_CUSTOM_NUMERIC_TYPES=(float)(int64_t)"
)

set_target_properties(implot-lib
    PROPERTIES
        CXX_STANDARD 20
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS NO
        POSITION_INDEPENDENT_CODE ON
)


if(SANITIZE)
    if(${SANITIZE} STREQUAL "ubsan")
        target_compile_options(implot-lib
            PRIVATE
                -fsanitize=undefined -fno-sanitize-recover=undefined
        )
        target_link_options(implot-lib
            PRIVATE
                -fsanitize=undefined -fno-sanitize-recover=undefined
        )
    endif()
endif()
















