#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <fstream>

using namespace std;

// 함수 선언
void generateMatrix(vector<vector<int>>& matrix, int rows, int cols);
void printMatrix(const vector<vector<int>>& matrix);
vector<vector<int>> addMatrices(const vector<vector<int>>& matrix1, const vector<vector<int>>& matrix2);
vector<vector<int>> multiplyMatrices(const vector<vector<int>>& matrix1, const vector<vector<int>>& matrix2);
vector<vector<int>> transposeMatrix(const vector<vector<int>>& matrix);
void saveToFile(const string& filename, const vector<vector<int>>& matrix1, const vector<vector<int>>& matrix2, const vector<vector<int>>& sum, const vector<vector<int>>& product, const vector<vector<int>>& transpose1, const vector<vector<int>>& transpose2);
void readFromFile(const string& filename);

int main() {
    srand(time(0));

    int rows, cols;
    cout << "Enter the number of rows: ";
    cin >> rows;
    cout << "Enter the number of columns: ";
    cin >> cols;

    vector<vector<int>> matrix1(rows, vector<int>(cols));
    vector<vector<int>> matrix2(rows, vector<int>(cols));

    generateMatrix(matrix1, rows, cols);
    generateMatrix(matrix2, rows, cols);

    vector<vector<int>> sum = addMatrices(matrix1, matrix2);
    vector<vector<int>> product = multiplyMatrices(matrix1, matrix2);
    vector<vector<int>> transpose1 = transposeMatrix(matrix1);
    vector<vector<int>> transpose2 = transposeMatrix(matrix2);

    saveToFile("matrix_results.txt", matrix1, matrix2, sum, product, transpose1, transpose2);
    readFromFile("matrix_results.txt");

    return 0;
}

void generateMatrix(vector<vector<int>>& matrix, int rows, int cols) {
    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            matrix[i][j] = rand() % 100;
        }
    }
}

void printMatrix(const vector<vector<int>>& matrix) {
    for (const auto& row : matrix) {
        for (int val : row) {
            cout << val << " ";
        }
        cout << endl;
    }
}

vector<vector<int>> addMatrices(const vector<vector<int>>& matrix1, const vector<vector<int>>& matrix2) {
    int rows = matrix1.size();
    int cols = matrix1[0].size();
    vector<vector<int>> result(rows, vector<int>(cols));

    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            result[i][j] = matrix1[i][j] + matrix2[i][j];
        }
    }

    return result;
}

vector<vector<int>> multiplyMatrices(const vector<vector<int>>& matrix1, const vector<vector<int>>& matrix2) {
    int rows = matrix1.size();
    int cols = matrix2[0].size();
    int inner = matrix2.size();
    vector<vector<int>> result(rows, vector<int>(cols, 0));

    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            for (int k = 0; k < inner; ++k) {
                result[i][j] += matrix1[i][k] * matrix2[k][j];
            }
        }
    }

    return result;
}

vector<vector<int>> transposeMatrix(const vector<vector<int>>& matrix) {
    int rows = matrix.size();
    int cols = matrix[0].size();
    vector<vector<int>> result(cols, vector<int>(rows));

    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            result[j][i] = matrix[i][j];
        }
    }

    return result;
}

void saveToFile(const string& filename, const vector<vector<int>>& matrix1, const vector<vector<int>>& matrix2, const vector<vector<int>>& sum, const vector<vector<int>>& product, const vector<vector<int>>& transpose1, const vector<vector<int>>& transpose2) {
    ofstream file(filename);

    file << "Matrix 1:\n";
    for (const auto& row : matrix1) {
        for (int val : row) {
            file << val << " ";
        }
        file << endl;
    }
    file << "\nMatrix 2:\n";
    for (const auto& row : matrix2) {
        for (int val : row) {
            file << val << " ";
        }
        file << endl;
    }
    file << "\nSum of matrices:\n";
    for (const auto& row : sum) {
        for (int val : row) {
            file << val << " ";
        }
        file << endl;
    }
    file << "\nProduct of matrices:\n";
    for (const auto& row : product) {
        for (int val : row) {
            file << val << " ";
        }
        file << endl;
    }
    file << "\nTranspose of Matrix 1:\n";
    for (const auto& row : transpose1) {
        for (int val : row) {
            file << val << " ";
        }
        file << endl;
    }
    file << "\nTranspose of Matrix 2:\n";
    for (const auto& row : transpose2) {
        for (int val : row) {
            file << val << " ";
        }
        file << endl;
    }

    file.close();
}

void readFromFile(const string& filename) {
    ifstream file(filename);
    string line;

    while (getline(file, line)) {
        cout << line << endl;
    }

    file.close();
}
