# Libft
> My First C Library

- [Libft](#libft)
	- [MANDATORY](#mandatory)
		- [Makefile](#makefile)
		- [Lib functions](#lib-functions)
			- [ft\_is~ series](#ft_is-series)
			- [ft\_strlen](#ft_strlen)
			- [ft\_memset](#ft_memset)
			- [ft\_bzero](#ft_bzero)
			- [ft\_memcpy](#ft_memcpy)
			- [ft\_memmove](#ft_memmove)
			- [ft\_strlcpy](#ft_strlcpy)
			- [ft\_strlcat](#ft_strlcat)
			- [ft\_strncmp](#ft_strncmp)
			- [ft\_to~ series](#ft_to-series)
			- [ft\_strchr](#ft_strchr)
			- [ft\_strrchr](#ft_strrchr)
			- [ft\_memchr](#ft_memchr)
			- [ft\_memcmp](#ft_memcmp)
			- [ft\_strnstr](#ft_strnstr)
			- [ft\_atoi](#ft_atoi)
			- [ft\_calloc](#ft_calloc)
			- [ft\_strdup](#ft_strdup)
		- [Additional functions](#additional-functions)
			- [ft\_substr](#ft_substr)
			- [ft\_strjoin](#ft_strjoin)
			- [ft\_strtrim](#ft_strtrim)
			- [ft\_split](#ft_split)
			- [ft\_itoa](#ft_itoa)
			- [ft\_strmapi](#ft_strmapi)
			- [ft\_striteri](#ft_striteri)
			- [ft\_putchar\_fd](#ft_putchar_fd)
			- [ft\_putstr\_fd](#ft_putstr_fd)
			- [ft\_putendl\_fd](#ft_putendl_fd)
			- [ft\_putnbr\_fd](#ft_putnbr_fd)
	- [BONUS](#bonus)
		- [Makefile](#makefile-1)
		- [Bonus functions](#bonus-functions)
			- [ft\_lstnew](#ft_lstnew)
			- [ft\_lstadd\_front](#ft_lstadd_front)
			- [ft\_lstsize](#ft_lstsize)
			- [ft\_lstlast](#ft_lstlast)
			- [ft\_lstadd\_back](#ft_lstadd_back)
			- [ft\_lstdelone](#ft_lstdelone)
			- [ft\_lstclear](#ft_lstclear)
			- [ft\_lstiter](#ft_lstiter)
			- [ft\_lstmap](#ft_lstmap)
		- [Reference](#reference)

<br>

## MANDATORY

### Makefile

`man ar`, `man cc`

### Lib functions

#### ft_is~ series
```c
int		ft_isalpha(int c);
int		ft_isdigit(int c);
int		ft_isalnum(int c);
int		ft_isascii(int c);
int		ft_isprint(int c);
```
매개변수 c가 아스키 코드상으로 조건에 맞으면 참(1), 조건에 맞지 않으면 거짓(0)을 반환하는 함수이다.<br>
조건문만 사용하면 되는 함수였다.<br>
~~문자, <= 를 이용해서 좀 더 명시적이고 가독성을 높일 수 있었으나 <= 는 < && == 연산이기 때문에 연산을 두 번한다는 단점이 있었다.<br>
어차피 계속해서 사용할 함수이기 때문에 성능 최적화를 위해 아스키 코드와 < 만을 이용하여 함수를 구성했다.~~<br>
다시 보니까 가독성이 떨어지는 부분때문에 다시 수정했다.<br>
요즘의 하드웨어에서는 가독성이 더 중요할 것이다.

#### ft_strlen
```c
size_t	ft_strlen(const char *s);
```

[size_t](https://github.com/seungwonme/42_Seoul/issues/1) 자료형은 부호없는 정수 자료형이면서 운영체제에 맞는 고정된 크기를 갖기 때문에 size_t를 사용하는 것 같다.<br>

#### ft_memset
```c
void	*ft_memset(void *b, int c, size_t len);
```

c를 unsigned char로 형변환한 값을 b의 값에 len만큼 넣는 함수이다.<br>

#### ft_bzero
```c
void	ft_bzero(void *s, size_t n);
```

s가 가리키는 주소를 0으로 n번만큼 바꾼다.
char를 사용한 이유는 1바이트단위로 주소를 참조하기 위해서이며, 밑의 함수에서도 같은 이유로 사용한다.

#### ft_memcpy
```c
void	*ft_memcpy(void *dst, const void *src, size_t n);
```

src의 값을 dst로 n바이트만큼 복사한다.<br>
src와 dst 값이 겹친다면 동작하지 않는다.<br>
dst, src를 형변환하여 복사하려 했으나, invalid type conversion이라는 오류 때문에 같은 주소를 가리키는 <br>char 포인터 변수를 사용하며 복사해주었다.<br>
여기서 무엇을 실수했는가?<br>
`(char *)*(dst + i) = (char \*)\*(src + i);`<br>
형변환을 이런식으로 했다.<br>
문제점은 void 포인터 dst, src를 *로 먼저 참조를 했기 때문에 오류가 난것<br>
그렇기에 변수를 지우고 형변환하여 해결했다 !<br>
또한, 둘 다 널포인터일 때의 예외처리가 왜 필요한 지 모르겠지만 paco에서 하라길래 했다.<br>

#### ft_memmove
```c
void	*ft_memmove(void *dst, const void *src, size_t len);
```

src의 값을 dst로 n바이트만큼 복사한다.<br>
memcpy와의 차이점은 src와 dst 값이 겹쳐져도 복사한다.<br>
memcpy처럼 0번째 인덱스부터 순차적으로 복사했을 때,<br>
`src < dst` 라면 복사 중 src의 값이 변하기 때문에 온전히 복사하지 못할 수 있다.<br>
따라서, 마지막 인덱스부터 복사해주면 된다.<br>

#### ft_strlcpy
```c
size_t	ft_strlcpy(char *dst, const char *src, size_t dstsize);
```

문자열 src를 dst에 dstsize만큼 복사한다.<br>
이 때, dstsize번째에는 널문자를 넣어준다.<br>
반환값은 문자열 src의 길이<br>

#### ft_strlcat
```c
size_t	ft_strlcat(char *dst, const char *src, size_t dstsize);
```

문자열 dst의 끝에 src를 붙여서 dstsize 크기를 가진 문자열을 만든다. 만약 `ft_strlen(dst) >= dstsize`라면 함수는 동작하지 않고 `dstsize + ft_strlen(src)`를 반환한다. 나머지의 경우는 dstsize 만큼의 문자열을 만들고 `ft_strlen(dst) + ft_strlen(src)`를 반환한다.

#### ft_strncmp
```c
int		ft_strncmp(const char *s1, const char *s2, size_t n);
```

문자열 s1, s2를 n바이트만큼 탐색해서 만약 값이 s1 > s2 라면 양수를, s1 < s2 라면 음수를, 같다면 0을 반환한다. (다르다면 값의 차를 반환한다.)

#### ft_to~ series
```c
int		ft_toupper(int c);
int		ft_tolower(int c);
```

#### ft_strchr
```c
char	*ft_strchr(const char *s, int c);
```

int 자료형인 c를 char로 형변환한 값을 문자열 s에서 찾는다.
0번째 인덱스부터 널문자까지 탐색한다.
찾았다면 해당 문자의 주소를, 찾지 못한다면 널포인터를 반환한다.

#### ft_strrchr
```c
char	*ft_strrchr(const char *s, int c);
```

int 자료형인 c를 char로 형변환한 값을 s에서 찾는다.
널문자부터 0번째 인덱스까지 탐색한다.
찾았다면 해당 문자의 주소를, 찾지 못한다면 널포인터를 반환한다.

#### ft_memchr
```c
void	*ft_memchr(const void *s, int c, size_t n);
```

int 자료형인 c를 unsigned char로 형변환한 값을  s에서 찾는다.
0번째 인덱스부터 널문자까지 탐색한다.
찾았다면 해당 문자의 주소를, 찾지 못한다면 널포인터를 반환한다.

#### ft_memcmp
```c
int	ft_memcmp(const void *s1, const void *s2, size_t n);
```

s1, s2를 n바이트만큼 탐색해서 만약 값이 s1 > s2 라면 양수를, s1 < s2 라면 음수를, 같다면 0을 반환한다. (다르다면 값의 차를 반환한다.)

#### ft_strnstr
```c
char	*ft_strnstr(const char *haystack, const char *needle, size_t len);
```

문자열 haystack에 문자열 needle이 들어가 있는 지 n바이트만큼 탐색

#### ft_atoi
```c
int	ft_atoi(const char *str);
```

ascii to integer

#### ft_calloc
```c
void	*ft_calloc(size_t count, size_t size);
```

size * count 만큼 malloc 후, 0으로 초기화

#### ft_strdup
```c
char	*ft_strdup(const char *s1);
```

문자열 s1을 값을 복사하는 문자열 반환
이 때, malloc으로 할당해주기때문에 free는 함수 사용자의 몫

### Additional functions

#### ft_substr
```c
char	*ft_substr(char const *s, unsigned int start, size_t len);
```

#### ft_strjoin
```c
char	*ft_strjoin(char const *s1, char const *s2);
```

#### ft_strtrim
```c
char	*ft_strtrim(char const *s1, char const *set);
```

#### ft_split
```c
char	**ft_split(char const *s, char c);
```

라피신 때의 split과 동일하나, 만약 배열에 집어넣다가 할당에 실패하면 malloc한 주소들을 모두 free해주고 널포인터를 반환해야한다.

#### ft_itoa
```c
char	*ft_itoa(int n);
```

integer to ascii

#### ft_strmapi
```c
char	*ft_strmapi(char const *s, char (*f)(unsigned int, char));
```

#### ft_striteri
```c
void	ft_striteri(char *s, void (*f)(unsigned int, char*));
```

#### ft_putchar_fd
```c
void	ft_putchar_fd(char c, int fd);
```

#### ft_putstr_fd
```c
void	ft_putstr_fd(char *s, int fd);
```

#### ft_putendl_fd
```c
void	ft_putendl_fd(char *s, int fd);
```

#### ft_putnbr_fd
```c
void	ft_putnbr_fd(int n, int fd);
```

## BONUS
```Makefile
bonus:
	$(MAKE) 'MANDATORY_OBJS=$(MANDATORY_OBJS) $(BONUS_OBJS)' all
```

보너스 규칙으로는 MANDATORY_OBJS에 BONUS_OBJS를 추가해준다.

### Makefile

### Bonus functions

#### ft_lstnew
```c
t_list	*ft_lstnew(void *content);
```

#### ft_lstadd_front
```c
void	ft_lstadd_front(t_list **lst, t_list *new);
```

#### ft_lstsize
```c
int		ft_lstsize(t_list *lst);
```

#### ft_lstlast
```c
t_list	*ft_lstlast(t_list *lst);
```

#### ft_lstadd_back
```c
void	ft_lstadd_back(t_list **lst, t_list *new);
```

#### ft_lstdelone
```c
void	ft_lstdelone(t_list *lst, void (*del)(void *));
```

#### ft_lstclear
```c
void	ft_lstclear(t_list **lst, void (*del)(void *));
```

#### ft_lstiter
```c
void	ft_lstiter(t_list *lst, void (*f)(void *));
```

#### ft_lstmap
```c
t_list	*ft_lstmap(t_list *lst, void *(*f)(void *), void (*del)(void *));
```

### Reference

- [ar 명령어로 라이브러리 다루기](https://betterourlife.tistory.com/17)
