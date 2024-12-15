# среда программирования
Windows 11，VScode，Используйте язык С++

Установите MinGW-w64 для использования g++.

https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/seh/x86_64-8.1.0-release-posix-seh-rt_v6-rev0.7z
# Формат хранения матрицы
Все матрицы являются разреженными. Матрицы хранятся в файлах формата Matrix Market (.mtx).
![image](https://github.com/user-attachments/assets/e6e0fa85-f585-4199-b64c-e0907481f1e4)

Матрица сохраняется в формате CSR во время работы программы.
# Введение в программу
Всего процедур четыре:

generate_sparse_matrix используется для генерации разреженной матрицы и сохранения ее в формате .mtx. Используемый формат ввода: «Имя файла, в котором хранится матрица» «Количество строк матрицы», «Количество столбцов матрицы», «Количество ненулевых элементов». Заполненное значение представляет собой один десятичный знак от 0 до 100.

matrix_multiplication используется для вычисления умножения двух разреженных матриц и сохранения результатов вычислений в формате .mtx. Используемый входной формат: «имя файла матрицы 1», «имя файла матрицы 2», «имя файла выходной матрицы».

generate_sparse_matrix_parallel и matrix_multiplication_parallel — это распараллеленные версии двух вышеупомянутых программ, использующие openmp соответственно.
# Отображение результатов
При параллельной генерации разреженных матриц каждый поток отвечает за определенный диапазон строк (от start_row до end_row), что гарантирует, что позиции элементов, сгенерированных разными потоками, не перекрываются. Каждый поток сохраняет сгенерированные элементы в local_elements и, наконец, использует #pragma omp critical, чтобы обеспечить потокобезопасное слияние local_elements с элементами. Ниже приводится сравнение времени работы:
![image](https://github.com/user-attachments/assets/a8f3d73b-cdfe-40fb-a5b6-2697086010f6)
![image](https://github.com/user-attachments/assets/e5666ee8-d1c7-4346-8259-4ddab1c27110)

Для умножения матриц распараллеленный код использует временные std::vector<std::vector<double>> и std::vector<std::vector<int>> вместо хэш-таблицы для хранения каждого ненулевого значения и индекс столбца строки, и использует несколько временных переменных, чтобы помочь вычислить и сохранить промежуточные результаты. #pragma omp Parallel и #pragma omp for Schedule(dynamic) используются во внешнем цикле для параллельной обработки вычислений каждой строки, а #pragma omp Critical используется для объединения результатов каждого потока для обеспечения безопасности потоков.Ниже приводится сравнение времени работы(Это умножение двух матриц размером 10 000 × 10 000, каждая из которых содержит 400 000 ненулевых элементов. Результат содержит 14 718 717 ненулевых значений):
![image](https://github.com/user-attachments/assets/e2a00738-fa9c-433a-953d-a3aca1f920c8)
![image](https://github.com/user-attachments/assets/ab29277b-6347-4397-9ade-c4d34729969e)
Файл результатов слишком большой, выложу частичный скриншот.
![image](https://github.com/user-attachments/assets/40a0c31a-f73c-4e6d-bf18-ff900d1f7811)





