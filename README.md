# Libft

The Libft project is part of the 42 school curriculum, designed to enhance fundamental C programming skills by implementing a custom C library. The project aims to deepen understanding of basic C programming and involve the direct implementation of standard library functions.

## Overview
This library is comprised of the following key components:

- String and character manipulation functions: ft_strlen, ft_strcpy, ft_strcmp, etc.
- Memory management functions: ft_memalloc, ft_memdel, ft_memcpy, etc.
- Character and string examination functions: ft_isalpha, ft_isdigit, ft_strchr, etc.
- List manipulation functions: ft_lstnew, ft_lstadd, ft_lstmap, etc.
- Additional functionality: Any additional functions as required by the project.

Each function has a signature similar to standard library functions and can be extended based on project requirements.

## Usage
1. Clone the project:
```bash
git clone https://github.com/seungwonme/libft
```

2. Navigate to the project directory:
```bash
cd libft && rm -rf .git
```

3. Compile the library:
```bash
make [bonus]
```

4. In your project, include the library's header file, and link the library during compilation.
```c
#include "libft.h"

int main(void)
{
    ft_putstr("Hello, Libft!\n");
    return 0;
}
```

- To clean up object files:
```bash
make clean
```

- To delete all build files:
```bash
make fclean
```

- To clean and rebuild the library:
```bash
make re
```
