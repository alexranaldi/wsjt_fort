# wsjt_fort

Fortran library from WSJT-X for native Windows projects (Windows, Visual Studio, x64)

The compiled binary for Windows x64, Visual Studio 2019, is in the "Release" folder.

This is essentially the "lib" folder from WSJT-X 2.3.0, with a few small modifications to make the Fortran code compilable with Intel® Fortran Compiler Classic 2021.2.0 [Intel(R) 64] in Visual Studio 2019.  Accordingly, the license follows that of WSJT-X, GPL v3.

The resulting binary is a static library that can be used in native Windows projects, and is easily callable from C or C++ code.

For example, to call FT8 functions to generate audio, declare the appropriate Fortran functions as extern in your C code:

```
typedef size_t fortran_charlen_t;

extern "C" {
    void GENFT8(char* msg, int* i3, int* n3, char* msgsent, char ft8msgbits[],
        int itone[], fortran_charlen_t, fortran_charlen_t);

    void GEN_FT8WAVE(int itone[], int* nsym, int* nsps, float* bt, float* fsample, float* f0,
        float xjunk[], float wave[], int* icmplx, int* nwave);
}
```

The functions should then be callable. For example:

```
int i3 = 0;
int n3 = 0;
char msgsent[37];
char ft8msgbits[77];
#define MAX_NUM_SYMBOLS 250
int itone[MAX_NUM_SYMBOLS] = {0};

GENFT8(message, &i3, &n3, msgsent, const_cast<char*> (ft8msgbits),
    const_cast<int*> (itone), 37, 37);
```
