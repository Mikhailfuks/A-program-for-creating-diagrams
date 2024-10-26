#include <iostream>
#include <vector>
#include <string>
#include <fstream>
#include <sstream>
#include <algorithm>
#include <cmath>

using namespace std;

// Структура для хранения данных диаграммы
struct ChartData {
    vector<string> labels;
    vector<double> values;
};

// Функция для чтения данных диаграммы из файла
ChartData readChartData(const string& filename) {
    ChartData data;
    ifstream file(filename);

    if (file.is_open()) {
        string line;
        while (getline(file, line)) {
            stringstream ss(line);
            string label, valueStr;
            getline(ss, label, ',');
            getline(ss, valueStr, ',');
            data.labels.push_back(label);
            data.values.push_back(stod(valueStr));
        }
        file.close();
    } else {
        cerr << "Ошибка открытия файла: " << filename << endl;
    }

    return data;
}

// Функция для вывода диаграммы в консоль
void printChart(const ChartData& data) {
    // Находим максимальное значение для масштабирования
    double maxValue = *max_element(data.values.begin(), data.values.end());

    // Определяем ширину диаграммы
    int chartWidth = 40;

    // Вывод заголовка диаграммы
    cout << "Диаграмма:" << endl;

    // Вывод оси Y
    for (int i = 0; i <= 5; i++) {
        cout << round(maxValue * i / 5) << " ";
        for (int j = 0; j < chartWidth; j++) {
            if (i == 5 || j == chartWidth - 1) {
                cout << "|";
            } else {
                cout << " ";
            }
        }
        cout << endl;
    }

    // Вывод оси X
    cout << " ";
    for (int i = 0; i < chartWidth; i++) {
        cout << "-";
    }
    cout << endl;

    // Вывод столбцов
    for (size_t i = 0; i < data.labels.size(); i++) {
        cout << data.labels[i] << ": ";

        // Вычисляем высоту столбца
        int barHeight = round(data.values[i] / maxValue * chartWidth);

        // Выводим столбец
        for (int j = 0; j < barHeight; j++) {
            cout << "*";
        }

        // Выводим пробелы до конца строки
        for (int j = barHeight; j < chartWidth; j++) {
            cout << " ";
        }

        cout << endl;
    }
}

int main() {
    string filename;

    cout << "Введите имя файла с данными диаграммы: ";
    cin >> filename;

    ChartData data = readChartData(filename);

    printChart(data);

    return 0;
}
