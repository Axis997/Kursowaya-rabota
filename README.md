# Kursowaya-rabota
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#include <cmath>
#include <iomanip>

std::vector<std::string> calculate_break_even_point(const std::vector<double>& data) {
    double fixed_costs = data[0];      // Постоянные затраты
    double selling_price = data[1];    // Цена реализации
    double var_cost_per_unit = data[2];// Переменные затраты на единицу
    double actual_vol = data[3];       // Фактический объем

    // --- 1. Точка безубыточности (в натуральном выражении) ---
    double be_raw = fixed_costs / (selling_price - var_cost_per_unit);
    int break_even_units = static_cast<int>(be_raw);
    if (be_raw != static_cast<double>(break_even_units)) {
        break_even_units++; // Округляем вверх
    }
    std::string be_str = "Точка безубыточности: " + std::to_string(break_even_units) + " ед.";

    // --- 2. Запас финансовой прочности (в %) ---
    double margin_pct = 0.0;
    if (actual_vol > 0) {
        margin_pct = ((actual_vol - break_even_units) / actual_vol) * 100.0;
    }
    std::stringstream ss1;
    ss1 << std::fixed << std::setprecision(2) << margin_pct;
    std::string margin_str = "Запас финансовой прочности: " + ss1.str() + "%";

    // --- 3. Операционный рычаг ---
    double gross_profit = (selling_price - var_cost_per_unit) * actual_vol;
    double op_income = gross_profit - fixed_costs;
    double op_leverage = 0.0;
    if (op_income > 0) { // Избегаем деления на 0 и отрицательную прибыль
        op_leverage = gross_profit / op_income;
    }
    std::stringstream ss2;
    ss2 << std::fixed << std::setprecision(2) << op_leverage;
    std::string lev_str = "Операционный рычаг: " + ss2.str();

    return {be_str, margin_str, lev_str};
}

int main() {
    std::vector<double> input(4); // Создаем вектор из 4 элементов

    std::cout << "Введите 4 значения (постоянные_затраты, цена_реализации, переменные_затраты_на_единицу, фактический_объем), разделенные пробелом: ";
    for (int i = 0; i < 4; ++i) {
        std::cin >> input[i];
    }

    auto result = calculate_break_even_point(input);

    for (const auto& line : result) {
        std::cout << line << std::endl;
    }

    return 0;
}
