# laba7
//Минимальное остовное дерево за O(NlogN) методом Краскала через DSU(систему непересекающихся множеств)
#include "stdafx.h"
#include <iostream>
#include <fstream>
#include "string.h"


using namespace std;

void creatFile(int maxCost, int kolVer, int **cost){
	//функция, создающая текстовый файл с заданным названием и записывающая в него данные для хранения
	int i, j;
	char name[50];
	cout << "Введите название файла:";
	cin >> name;
	strcat_s(name, ".txt");
	ofstream out(name);
	out << maxCost << "\n"; //Записываем в первую строку файла максимальный вес ребра
	out << kolVer << "\n";  //Записываем во вторую строку количество вершин

	//в следующии строки записываем матрицу смежности
	for (i = 0; i<kolVer; i++)
	{
		for (j = 0; j<kolVer; j++)
		{
			out << cost[i][j] << " ";
		}
		out << "\n";
	}

	out.close();
	cout << "Файл создан\n";
};
void poiskMinOstDer(int maxCost, int kolVer, int **cost)
{
	//Функциия поиска минимального оставного дерева на графе
	int min;
	int minCost = 0;
	int i, i2, i3;
	int j, j2, j3;
	int shet = 1, iRebra = 0;
	bool used[10] = { false };
	used[1] = true;
	int rebro[99] = { 0 };

	while (shet<kolVer){
		for (i = 1, min = maxCost; i <= kolVer; i++)
		{
			for (j = 1; j <= kolVer; j++)
			{
				if (cost[i][j]<min)
				{
					if (used[i] != false)
					{
						min = cost[i][j];
						i2 = i3 = i;
						j2 = j3 = j;
					}
				}
			}
		}
		if (used[i3] == false || used[j3] == false)
		{
			rebro[iRebra] = j2;
			cout << "\n Из " << i2 << " вершины в " << j2 << " вершину, ребро=" << min;
			iRebra++;
			shet++;
			minCost = minCost + min;
			used[j2] = true;
		}

		cost[i2][j2] = cost[j2][i2] = maxCost;
	}
	cout << "\n";
	cout << "\n Минимальная стоимость: " << minCost;
	cout << "\n";
};
int main(){
	setlocale(LC_ALL, "RUS");
	int i, j;
	int maxCost = 100; //Максимальный вес ребра
	int kolVer; //Количество вершин графа
	int **cost;
	cost = new int *[99];
	for (i = 1; i <= 99; i++){ cost[i] = new int[99]; }


	cout << "Введите максимальный вес ребра:\n";
	cin >> maxCost;
	cout << "Введите количесто вершин графа:\n";
	cin >> kolVer;

	// Записываем в cost вес ребер с помощью матрицы смежности
	cout << "Введите матрицу смежности:\n";
	for (i = 1; i <= kolVer; i++)
	{
		for (j = 1; j <= kolVer; j++)
		{
			cin >> cost[i][j];
			if (cost[i][j] == 0){ cost[i][j] = maxCost; }
		}
	}

	//creatFile(maxCost,kolVer,cost);
	poiskMinOstDer(maxCost, kolVer, cost);


	system("pause");
	return 0;
};
