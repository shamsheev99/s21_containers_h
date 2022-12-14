.PHONY: all clean test cpplint cppcheck leaks gcov_report rebuild

CC = g++
FLAGS = -Wall -Wextra -Werror -std=c++17
GTEST_FLAGS = -lgtest
GCOV_FLAG = -fprofile-arcs -ftest-coverage 
NAME = s21_containers
TESTFILE=test
LIB_NAME =


OS = $(shell uname)
ifeq ($(OS), Linux)
	LIBFLAGS=-lstdc++ `pkg-config --cflags --libs gtest`
else
	LIBFLAGS=-lstdc++ -lm -lgtest
endif

rebuild: clean all

check: cppcheck cpplint

all: $(TESTFILE) 


$(TESTFILE):
	$(CC) $(LIB_NAME) tests.cpp $(FLAGS) $(GCOV_FLAG) -o $(TESTFILE) $(LIBFLAGS)
	./$(TESTFILE)

gcov_report: $(TESTFILE)
	lcov --no-external -t "$(TESTFILE)" -o $(TESTFILE).info -c -d ./
	genhtml -o report $(TESTFILE).info
	open report/index.html

cpplint:
	@echo "****** linter start ******"
	cp ../materials/linters/.clang-format .
	clang-format -i *.h *.tpp
	clang-format -n *.h *.tpp

cppcheck:
	@echo "****** cppcheck start ******"
	cppcheck --enable=all --suppress=unmatchedSuppression  --suppress=missingIncludeSystem --language=c++ --std=c++17 *.cpp *.h	

clean:
	rm -f *.o *.a *.gcno *.gcda *.info a.out $(TESTFILE)
	rm -rf report/

leaks:
ifeq ($(OS), Linux)
	CK_FORK=no valgrind --tool=memcheck --leak-check=full --show-leak-kinds=all --show-reachable=yes ./$(TESTFILE)
else
	CK_FORK=no leaks -atExit -- ./$(TESTFILE)
endif
