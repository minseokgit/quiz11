#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"
#include <stdio.h>
#include <windows.h>

void displayImage(const char* filename) {
    int width, height, channels;
    unsigned char* img = stbi_load(filename, &width, &height, &channels, 0);
    if (img == NULL) {
        printf("Error in loading the image %s\n", filename);
        return;
    }
    printf("Loaded image %s with a width of %dpx, a height of %dpx and %d channels\n", filename, width, height, channels);

    // 화면에 출력
    HWND console = GetConsoleWindow();
    HDC dc = GetDC(console);

    for (int y = 0; y < height; y++) {
        for (int x = 0; x < width; x++) {
            int idx = (y * width + x) * channels;
            COLORREF color = (channels >= 3) ? RGB(img[idx], img[idx + 1], img[idx + 2]) : RGB(img[idx], img[idx], img[idx]);
            SetPixel(dc, x, y, color);
        }
    }

    ReleaseDC(console, dc);
    stbi_image_free(img);
}

int main() {
    displayImage("jcshim.png");
    printf("PNG image displayed. Press Enter to continue.\n");
    getchar();

    displayImage("jcshim.jpg");
    printf("JPG image displayed. Press Enter to exit.\n");
    getchar();

    return 0;
}
