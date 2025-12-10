#include <iostream>
#include <string.h>
#include <fstream>
#include <cstring>
using namespace std;

/* 课题名称：	学生成绩管理系统3.0
   小组成员：顾晓其20251002979、曾蔚弘20251003292
   分工情况：顾晓其（菜单，功能0、1、4、6、7、5的功能3）
			 曾蔚弘（功能2、3、5的功能1、2）
   班级：计算机2501
   完成时间：2025.12.6 */


struct student {
	long long num;
	char name[20];
	float score[4];
	float pingjun;
};

struct student stud[20];//每个学生的信息存到一个数组里
// 计算最值+平均分

void menu1() {
	std::cout << "*******************************************************" << endl;
	std::cout << "|             欢迎使用学生成绩管理系统3.0              |" << endl;
	std::cout << "*******************************************************" << endl;
	std::cout << "                                                       " << endl;
	std::cout << "                                                       " << endl;
	std::cout << "请问您是否选择导入学生成绩？（Yes/No）      " << endl;
	std::cout << "                                                       " << endl;

}

void menu2() {
	std::cout << "                                                                             " << endl;
	std::cout << "*****************************************************************************" << endl;
	std::cout << "|                              请选择你需要的功能                           |" << endl;
	std::cout << "*****************************************************************************" << endl;
	std::cout << "|  1、求单个学生所有科目成绩的平均分                                        |" << endl;
	std::cout << "|  2、求单科目所有学生成绩的平均分                                          |" << endl;
	std::cout << "|  3、成绩分析                                                              |" << endl;
	std::cout << "|  4、修改学生信息                                                          |" << endl;
	std::cout << "|  5、查找学生成绩                                                          |" << endl;
	std::cout << "|  6、输出成绩表单                                                          |" << endl;
	std::cout << "|  7、显示某门课程所有学生成绩                                              |" << endl;
	std::cout << "|  0、退出并将数据导入到新文件                                              |" << endl;
	std::cout << "                                                                             " << endl;

}

void getMaxMin(int n, student stud[], int sub, int& maxscore, int& minscore, double& avgscore) {
	maxscore = minscore = stud[0].score[sub];
	int sum = stud[0].score[sub];

	// 遍历所有学生计算最值和总分
	for (int i = 1; i < n; i++) {
		int s = stud[i].score[sub];
		sum += s;
		if (s > maxscore) {
			maxscore = s;
		}
		if (s < minscore) {
			minscore = s;
		}
	}

	avgscore = (double)sum / n;
}

void fun1(int n) {
	std::cout << "请问您需要求谁的所有科目成绩平均分" << endl;
	char x[20];//输入需要寻找的学生的姓名
	cin >> x;
	int k = -1;
	for (int i = 0; i < n; i++) {
		if (strcmp(x, stud[i].name) == 0) {
			k = i;
			break;
		}
	}//寻找对应学生

	if (k == -1) {
		std::cout << "对不起，未找到相应学生" << endl;
	}
	else {//计算平均分
		stud[k].pingjun = 0;
		for (int i = 0; i < 4; i++) {
			stud[k].pingjun += stud[k].score[i];
		}
		stud[k].pingjun = stud[k].pingjun / 4;
		std::cout << "                                                       " << endl;
		std::cout << "已找到！以下是为您找到的信息" << endl;
		std::cout << "姓名：" << "  " << stud[k].name << endl;
		std::cout << "学号：" << "  " << stud[k].num << endl;
		std::cout << "所有科目成绩平均分为：" << '\t' << stud[k].pingjun << endl;
	}

}

void fun2(int n) {  //接收数据 n和成绩 （功能2）求单科平均分 

	std::cout << "求数学成绩请输入1" << endl
		<< "求英语成绩请输入2" << endl <<
		"求程序设计成绩请输入3" << endl <<
		"求物理成绩请输入4" << endl;
	std::cout << "                                                       " << endl;
	std::cout << "您要求的科目是：" << endl;  //用户选择 //用switch判断求哪一个 

	int a = 0;
	cin >> a;
	int b;

	switch (a) {
	case 1:
		b = 0;
		break;//把b的值 
	case 2:
		b = 1;
		break;
	case 3:
		b = 2;
		break;
	case 4:
		b = 3;
		break;
	default:
		b = 4;
		break;//输入相应程序 
	}

	float dpinjun;

	if (b != 4) {                                                                                 //计算 d平均 
		dpinjun = 0;

		for (int i = 0; i < n; i++) {
			dpinjun += stud[i].score[b];
		}
		//b 的值是数组里 具体某一列,for语句累加 
		dpinjun = dpinjun / n;

		std::cout << "该科平均分为：" << dpinjun << endl;
	}
	else
		std::cout << "输入错误，退出该功能";//没有输入相应程序  跳转到这里 

}

void fun3(int n) {
	// 循环菜单,用了getmaxmin函数求最大值，小，平均。 
	while (true) {
		std::cout << "                                                       " << endl;
		cout << "------请选择您要分析的科目------" << endl;
		cout << "数学成绩请输入1" << endl;
		cout << "英语成绩请输入2" << endl;
		cout << "程序设计成绩请输入3" << endl;
		cout << "物理成绩请输入4" << endl;
		cout << "退出程序请输入5" << endl;
		std::cout << "                                                       " << endl;

		int a;
		cin >> a;
		int sub;  // 对应score数组的下标：0-3
		int maxscore, minscore;
		double avgscore;

		if (a == 5) {
			cout << "程序退出！" << endl;
			break; // 结束程序
		}
		else if (a == 1) {
			sub = 0;
			getMaxMin(n, stud, sub, maxscore, minscore, avgscore);
			cout << "数学成绩情况如下：" << endl;
		}
		else if (a == 2) {
			sub = 1;
			getMaxMin(n, stud, sub, maxscore, minscore, avgscore);
			cout << "英语成绩情况如下：" << endl;
		}
		else if (a == 3) {
			sub = 2;
			getMaxMin(n, stud, sub, maxscore, minscore, avgscore);
			cout << "程序设计成绩情况如下：" << endl;
		}
		else if (a == 4) {
			sub = 3;
			getMaxMin(n, stud, sub, maxscore, minscore, avgscore);
			cout << "物理成绩情况如下：" << endl;
		}
		else {
			cout << "输入错误！请重新输入" << endl;
			continue;
		}

		// 输出结果
		cout << "最高分为：" << maxscore << "分" << endl;
		cout << "最低分为：" << minscore << "分" << endl;
		cout << "平均分为：" << avgscore << "分" << endl << endl;
	}
}

void fun4(int n) {
	std::cout << "                                                  " << endl;
	std::cout << "请输入你要修改的学生姓名" << endl;
	char x[20];
	cin >> x;
	int decision = 0;
	int d;
	for (int i = 0; i < n; i++) {
		if (strcmp(x, stud[i].name) == 0) {
			std::cout << "请问您需要修改该学生的什么信息？" << endl;
			decision = 1;
			d = i;
		}
	}
	if (decision == 1) {
		while (true) {
			std::cout << "                                                       " << endl;
			std::cout << "修改学生学号输入 1 " << endl;
			std::cout << "修改学生姓名输入 2 " << endl;
			std::cout << "修改学生数学成绩输入 3 " << endl;
			std::cout << "修改学生英语成绩输入 4 " << endl;
			std::cout << "修改学生程序设计成绩输入 5 " << endl;
			std::cout << "修改学生物理成绩输入 6 " << endl;
			std::cout << "修改完成，退出功能请输入 0 " << endl;
			std::cout << "                                                       " << endl;

			int a;
			cin >> a;
			switch (a) {
			case 0:
				std::cout << "您已退出" << endl << endl;
				break;
			case 1:
				long long newnum;
				cout << "您需要修改的学号是：" << stud[d].num << endl;
				cout << "请输入新学号：" << endl;
				cin >> newnum;
				stud[d].num = newnum;
				cout << "修改成功！修改后的新学号：" << stud[d].num << endl;
				break;
			case 2:
				char newname[20];
				cout << "您需要修改的姓名是：" << stud[d].name << endl;
				cout << "请输入新姓名：" << endl;
				cin >> newname;
				strcpy_s(stud[d].name, sizeof(stud[d].name), newname);
				cout << "修改成功！修改后的新姓名：" << stud[d].name << endl;
				break;
			case 3:
				int newmath_score;
				cout << "您需要修改的是数学成绩：" << stud[d].score[0] << endl;
				cout << "请输入新的数学成绩：" << endl;
				cin >> newmath_score;
				stud[d].score[0] = newmath_score;
				cout << "修改成功！新的数学成绩是：" << stud[d].score[0] << endl;
				break;
			case 4:
				int neweng_score;
				cout << "您需要修改的是英语成绩：" << stud[d].score[1] << endl;
				cout << "请输入新的英语成绩：" << endl;
				cin >> neweng_score;
				stud[d].score[1] = neweng_score;
				cout << "修改成功！新的英语成绩是：" << stud[d].score[1] << endl;
				break;
			case 5:
				int newcscore;
				cout << "您需要修改的是程序设计成绩：" << stud[d].score[2] << endl;
				cout << "请输入新的程序设计成绩：" << endl;
				cin >> newcscore;
				stud[d].score[2] = newcscore;
				cout << "修改成功！新的程序设计成绩是：" << stud[d].score[2] << endl;
				break;
			case 6:
				int newphscore;
				cout << "您需要修改的是物理成绩：" << stud[d].score[3] << endl;
				cout << "请输入新的物理成绩：" << endl;
				cin >> newphscore;
				stud[d].score[3] = newphscore;
				cout << "修改成功！新的物理成绩是：" << stud[d].score[3] << endl;
				break;

			default:
				std::cout << "您的选择有误，请重新选择" << endl;
				break;
			}

			if (a == 0) {
				for (int k = 0; k < n; k++) {
					stud[k].pingjun = 0;
					for (int i = 0; i < 4; i++) {
						stud[k].pingjun += stud[k].score[i];

					}
					stud[k].pingjun = stud[k].pingjun / 4.0;
				}
				break;

			}

		}
	}
	else if (decision == 0)
		std::cout << "抱歉，未找到您需要修改的学生信息" << endl <<
		"退出功能，请重新选择" << endl << endl;

}

void fun5(int n) {

	while (true) {
		std::cout << "                                                  " << endl;
		std::cout << "--------------------------请选择查找学生成绩的方式----------------------" << endl;
		std::cout << "                              按学号查找请输入 1                              " << endl;
		std::cout << "                              按姓名查找请输入 2                 " << endl;
		std::cout << "                      查找总成绩平均分最高的学生请输入 3       " << endl;
		std::cout << "                              退出查找请输入 0                  " << endl;
		std::cout << "-----------------------------------------------------------------------------" << endl;
		long long  a;
		cin >> a;

		if (a == 1)//学号查找 
		{
			long long b;
			int search = 0;
			std::cout << "请输入该学生的学号：" << endl;
			cin >> b;//输入学号，按for语句循环匹配 
			for (int i = 0; i < n; i++)
			{
				if (b == stud[i].num)
				{

					std::cout << "学号查找成功！该学生的成绩情况是：" << endl;
					std::cout << "姓名：" << stud[i].name << endl;
					std::cout << "学号：" << stud[i].num << endl;
					std::cout << "数学成绩：" << stud[i].score[0] << endl;
					std::cout << "英语成绩：" << stud[i].score[1] << endl;
					std::cout << "程序设计成绩：" << stud[i].score[2] << endl;
					std::cout << "物理成绩：" << stud[i].score[3] << endl;
					std::cout << "平均成绩：" << stud[i].pingjun << endl;
					search = 1;
					//表明查找成功 ,用search判断 
					break;
				}


			}
			if (search == 0)//没有匹配的学号 
			{
				std::cout << "查找失败，请重新选择查找方式" << endl;
				continue;
			}
		}


		else if (a == 2)// 姓名查找
		{
			int search = 0;
			std::cout << "请输入该学生的姓名：" << endl;
			char namesearch[20];
			cin >> namesearch;
			for (int i = 0; i < n; i++)
			{

				if (strcmp(namesearch, stud[i].name) == 0)//相等 
				{
					std::cout << "姓名查找成功！该学生的成绩情况是：" << endl;
					std::cout << "姓名：" << stud[i].name << endl;
					std::cout << "学号：" << stud[i].num << endl;
					std::cout << "数学成绩：" << stud[i].score[0] << endl;
					std::cout << "英语成绩：" << stud[i].score[1] << endl;
					std::cout << "程序设计成绩：" << stud[i].score[2] << endl;
					std::cout << "物理成绩：" << stud[i].score[3] << endl;
					std::cout << "平均成绩：" << stud[i].pingjun << endl;
					std::cout << "                                                  " << endl;
					search = 1;
				}

			}
			if (search == 0) {
				std::cout << "查找失败，请重新选择查找方式" << endl;
				std::cout << "                                                  " << endl;
				continue;
			}

		}
		else if (a == 3) {
			float highscore = 0;
			int q = -1;
			for (int h = 0; h < n - 1; h++) {
				stud[h].pingjun <= stud[h + 1].pingjun ? highscore = stud[h + 1].pingjun : highscore = stud[h].pingjun;
			}
			for (int h = 0; h < n; h++) {
				if (highscore == stud[h].pingjun) {
					q = h;
				}
			}
			std::cout << "                                                  " << endl;
			cout << "已找到，最高平均分是：" << endl <<
				"姓名：" << stud[q].name << endl <<
				"学号：" << stud[q].num << endl <<
				"数学成绩：" << stud[q].score[0] << endl <<
				"英语成绩：" << stud[q].score[1] << endl <<
				"程序设计成绩：" << stud[q].score[2] << endl
				<< "物理成绩：" << stud[q].score[3] << endl;
			std::cout << "总成绩平均分是：" << stud[q].pingjun << endl;
		}
		else if (a == 0)//退出 
		{
			break;
		}
		else {
			std::cout << "-----------------------输入错误！请重新输入--------------------------" << endl;
			std::cout << "                                                  " << endl;
			continue;//返回选择方式 
		}

	}
}

void fun6(int n) {
	std::cout << "                                                  " << endl;
	std::cout << "*****************************************************************************" << endl;
	std::cout << "                                 总成绩表单                                  " << endl;
	std::cout << "*****************************************************************************" << endl;
	std::cout << "   学号      姓名     数学成绩    英语成绩   程序设计成绩   物理成绩   平均分" << endl; //输出学生成绩信息
	std::cout << "-----------------------------------------------------------------------------" << endl;
	for (int i = 0; i < n; i++) {
		std::cout << stud[i].num << " " << stud[i].name << '\t' << stud[i].score[0] << "           " << stud[i].score[1] << "          " << stud[i].score[2] << "            " << stud[i].score[3] << "       " << stud[i].pingjun << endl;
	}//输出每个学生的成绩

	float highscore = 0;//计算总成绩最高分
	int q = -1;
	for (int h = 0; h < n - 1; h++) {
		stud[h].pingjun <= stud[h + 1].pingjun ? highscore = stud[h + 1].pingjun : highscore = stud[h].pingjun;
	}
	for (int h = 0; h < n; h++) {
		if (highscore == stud[h].pingjun) {
			q = h;
		}
	}
	std::cout << "-----------------------------------------------------------------------------" << endl;
	std::cout << "平均分最高的是：" << stud[q].name << endl;
	std::cout << "*****************************************************************************" << endl;


}

void fun7(int n) {

	while (true) {
		cout << "请问您需要哪门课程的学生成绩？" << endl;
		cout << "数学成绩输入 1 " << endl << "英语成绩输入 2 " << endl << "程序设计成绩输入 3 " << endl << "物理成绩输入 4 " << endl << "退出请输入0" << endl;
		int a = 0;
		int b = -1;
		cin >> a;

		switch (a) {
		case 1:
			b = 0;
			break;
		case 2:
			b = 1;
			break;
		case 3:
			b = 2;
			break;
		case 4:
			b = 3;
			break;
		case 0:
			break;
		default:
			b = 4;
			cout << "选择有误，请重新选择！" << endl;
			break;
		}

		if (b != -1 && b != 4) {
			cout << "成绩情况如下：" << endl;
			for (int i = 0; i < n; i++) {
				cout << stud[i].name << ": " << stud[i].score[b] << endl;
			}
		}
		if (b == -1) {
			break;
		}
	}

}

int main() {
	ifstream infile;
	ofstream outfile;
	bool count = false;
	int n;
	int b = -1;
	string a;//’Yes/No‘判断变量'

	while (true) {

		menu1();//输出菜单1
		cin >> a;

		if (a == "Yes" || a == "YES" || a == "yes") {

			infile.open("cj11.txt");//打开文件

			if (!infile) {
				cout << "抱歉！文件打开失败，请检查文件路径" << endl;
				continue;
			}
			else {               //有多少个学生的信息
				infile >> n;
				count = true;
				break;
			}


		}

		if (a == "No" || a == "NO" || a == "no") {
			std::cout << "             欢迎您再次使用学生成绩管理系统                " << endl;
			break;
		}

	}

	while (count) {

		for (int i = 0; i < n; i++) {
			infile >> stud[i].num >> stud[i].name >> stud[i].score[0] >> stud[i].score[1] >> stud[i].score[2] >> stud[i].score[3];
		}//输入学生信息

		for (int k = 0; k < n; k++) {
			for (int i = 0; i < 4; i++) {
				stud[k].pingjun += stud[k].score[i];

			}
			stud[k].pingjun = stud[k].pingjun / 4.0;
		}
		//计算学生的平均分

		std::cout << "                                                       " << endl;
		std::cout << "          已录入，以下是录入学生的成绩信息             " << endl;
		std::cout << "   学号      姓名     数学成绩    英语成绩   程序设计成绩   物理成绩   " << endl;
		for (int i = 0; i < n; i++) {
			std::cout << stud[i].num << " " << stud[i].name << '\t' << stud[i].score[0] << "           " << stud[i].score[1] << "          " << stud[i].score[2] << "            " << stud[i].score[3] << endl;
		}
		std::cout << "                                                       " << endl;                   //输出学生成绩信息


		while (1) {
			menu2();
			cin >> b;//用户进行选择
			if (b == 1) {
				fun1(n);
			}
			else if (b == 2) {
				fun2(n);
			}
			else if (b == 3) {
				fun3(n);
			}
			else if (b == 4) {
				fun4(n);
			}
			else if (b == 5) {
				fun5(n);
			}
			else if (b == 6) {
				fun6(n);
			}
			else if (b == 7) {
				fun7(n);
			}
			else if (b == 0) {
				outfile.open("cj12.txt");
				if (!outfile) {
					cout << "文件打开失败！" << endl;
				}
				else {
					cout << "文件打开成功!已录入！" << endl;
				}
				outfile << "                                                  " << endl;
				outfile << "*****************************************************************************" << endl;
				outfile << "                                 总成绩表单                                  " << endl;
				outfile << "*****************************************************************************" << endl;
				outfile << "   学号      姓名     数学成绩    英语成绩   程序设计成绩   物理成绩   平均分" << endl; //输出学生成绩信息
				outfile << "-----------------------------------------------------------------------------" << endl;
				for (int i = 0; i < n; i++) {
					outfile << stud[i].num << " " << stud[i].name << '\t' << stud[i].score[0] << "           " << stud[i].score[1] << "          " << stud[i].score[2] << "            " << stud[i].score[3] << "       " << stud[i].pingjun << endl;
				}//输入每个学生的成绩

				float highscore = 0;//计算总成绩最高分
				int q = -1;
				for (int h = 0; h < n - 1; h++) {
					stud[h].pingjun <= stud[h + 1].pingjun ? highscore = stud[h + 1].pingjun : highscore = stud[h].pingjun;
				}
				for (int h = 0; h < n; h++) {
					if (highscore == stud[h].pingjun) {
						q = h;
					}
				}
				outfile << "-----------------------------------------------------------------------------" << endl;
				outfile << "平均分最高的是：" << stud[q].name << endl;
				outfile << "*****************************************************************************" << endl;
				std::cout << "          欢迎您再次使用学生成绩管理系统        " << endl;
				break;
			}
			else {
				cout << "选择错误！请再次选择！" << endl;
				continue;
			}
		}
		if (b == 0)
			break;
	}

	infile.close();
	outfile.close();
	return 0;
}
