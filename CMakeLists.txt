
project(implot)


include(FetchContent)

FetchContent_Declare(imgui
    GIT_REPOSITORY
        https://github.com/bostick/imgui.git
    GIT_TAG
        bostick-main
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
















