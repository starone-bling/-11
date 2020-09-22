//广度优先
#include<stdio.h>
struct node
{
    int xy[3][3];
    int dir;
};
struct node sh[102], end;
int count = 1;

void init()
{
    printf("输入起始节点的位置:n");
    int i, j;
    for (i = 0; i < 3; i++)
        for (j = 0; j < 3; j++)
            scanf_s("%d", &sh[0].xy[i][j]);
    sh[0].dir = -1;
    printf("输入目标节点的位置:n");
    for (i = 0; i < 3; i++)
        for (j = 0; j < 3; j++)
            scanf_s("%d", &sh[101].xy[i][j]);
    sh[101].dir = -1;
}
int loction(int num)
{
    int i;
    for (i = 0; i < 9; i++)
        if (sh[num].xy[i / 3][i % 3] == 0) return i;
}
long long sign(int num)
{
    long long  sum;
    sum = sh[num].xy[0][0] * 100000000 + sh[num].xy[0][1] * 10000000 + sh[num].xy[0][2] * 1000000 + sh[num].xy[1][0] * 100000 + sh[num].xy[1][1] * 10000 + sh[num].xy[1][2] * 1000 + sh[num].xy[2][0] * 100 + sh[num].xy[2][1] * 10 + sh[num].xy[2][2];
    return sum;
}

void mobile(int num)
{

    int temp;
    int loc;
    int up = 1, down = 1, left = 1, right = 1;
    loc = loction(num);
    int stand = sh[num].dir;
    if (loc / 3 != 0 && stand != 1)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3 - 1][loc % 3];
        sh[count].xy[loc / 3 - 1][loc % 3] = temp;
        sh[count].dir = 3;
        count++;
    };
    if (loc / 3 != 2 && stand != 3)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3 + 1][loc % 3];
        sh[count].xy[loc / 3 + 1][loc % 3] = temp;
        sh[count].dir = 1;
        count++;
    }
    if (loc % 3 != 0 && stand != 0)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3][loc % 3 - 1];
        sh[count].xy[loc / 3][loc % 3 - 1] = temp;
        sh[count].dir = 2;
        count++;
    }
    if (loc % 3 != 2 && stand != 2)
    {
        sh[count] = sh[num];
        temp = sh[count].xy[loc / 3][loc % 3];
        sh[count].xy[loc / 3][loc % 3] = sh[count].xy[loc / 3][loc % 3 + 1];
        sh[count].xy[loc / 3][loc % 3 + 1] = temp;
        sh[count].dir = 0;
        count++;
    }

}
void display(int num)
{
    int i, j;
    for (i = 0; i < 3; i++)
    {
        for (j = 0; j < 3; j++)
            printf("%d ", sh[num].xy[i][j]);
        printf("n");
    }
}

int search()
{
    int i = 0;
    while (1)
    {
        printf("n");
        display(i);
        printf("n");
        if (i == 100)
        {
            printf("超出了上限次数n");
            return 0;
        }
        if (sign(i) == sign(101))
        {
            printf("在第%d次找到了", i);
            display(i);
            return i;
        }
        mobile(i);
        i++;
    }
}

int main()
{
    init();
    search();
    return 0;
}
//深度优先
#include<iostream>
#include"queue"
#include"string"
#include<list>
using namespace std;
const string GOAL = "803214765";
class Situation {
private:

public:
    string father;
    string code;
    int deep;
    Situation up();
    Situation down();
    Situation left();
    Situation right();
    bool isGoal();
    bool isInOpen(deque<Situation>& open);
    bool isInClosed(deque<Situation>& closed);
    void show() const;
    void show(string) const;
    void show_deque(deque<Situation>&) const;
    deque<Situation> showWay(deque<Situation>& closed);
    void showAnswer(deque<Situation>& closed);
    Situation() :father(""), code(""), deep(-1) {};
};
#include"Deep.h"
Situation Situation::up() {
    string::size_type loc = code.find('0');
    Situation son;
    son.code = code;
    son.deep = deep + 1;
    if (loc >= 3) {
        char temp = son.code[loc];
        son.code[loc] = son.code[loc - 3];
        son.code[loc - 3] = temp;
    }
    else {
        son.code = "";
    }
    return son;
}
Situation Situation::down() {
    string::size_type loc = code.find('0');
    Situation son;
    son.code = code;
    son.deep = deep + 1;
    if (loc <= 5) {
        char temp = son.code[loc];
        son.code[loc] = son.code[loc + 3];
        son.code[loc + 3] = temp;
    }
    else {
        son.code = "";
    }
    return son;
}
Situation Situation::left() {
    string::size_type loc = code.find('0');
    Situation son;
    son.code = code;
    son.deep = deep + 1;
    if (loc != 0 && loc != 3 && loc != 6) {
        char temp = son.code[loc];
        son.code[loc] = son.code[loc - 1];
        son.code[loc - 1] = temp;
    }
    else {
        son.code = "";
    }
    return son;
}
Situation Situation::right() {
    string::size_type loc = code.find('0');
    Situation son;
    son.code = code;
    son.deep = deep + 1;
    if (loc != 2 && loc != 5 && loc != 8) {
        char temp = son.code[loc];
        son.code[loc] = son.code[loc + 1];
        son.code[loc + 1] = temp;
    }
    else {
        son.code = "";
    }
    return son;
}
bool Situation::isGoal() {
    return code == GOAL;
}
bool Situation::isInOpen(deque<Situation>& open) {
    for (int i = 0; i < open.size(); i++) {
        if (code == open.at(i).code) {
            return true;
        }
    }
    return false;
}
bool Situation::isInClosed(deque<Situation>& closed) {
    for (int i = 0; i < closed.size(); i++) {
        if (code == closed.at(i).code) {
            return true;
        }
    }
    return false;
}
void Situation::show() const {
    if (!code.empty()) {
        cout << code[0] << code[1] << code[2] << endl
            << code[3] << code[4] << code[5] << endl
            << code[6] << code[7] << code[8] << endl << endl;
    }
    else {
        cout << "空的" << endl;
    }
}
void Situation::show(string code) const {
    if (!code.empty()) {
        cout << code[0] << code[1] << code[2] << endl
            << code[3] << code[4] << code[5] << endl
            << code[6] << code[7] << code[8] << endl << endl;
    }
    else {
        cout << "空的" << endl;
    }
}
void Situation::show_deque(deque<Situation>& m_deque) const {
    for (int i = 0; i < m_deque.size(); i++) {
        m_deque.at(i).show();
    }
}
deque<Situation> Situation::showWay(deque<Situation>& closed) {
    //cout << closed.size() << endl;
    deque<Situation> dequeList;
    Situation temp = closed.back();
    dequeList.push_back(temp);
    for (int i = closed.size() - 1; i >= 0; i--) {
        if (temp.father == closed.at(i).code) {
            dequeList.push_back(closed.at(i));
            temp = closed.at(i);
        }
    }
    //cout << dequeList.size() << endl;
    return dequeList;
}
void Situation::showAnswer(deque<Situation>& closed) {
    deque<Situation> way(showWay(closed));
    cout << "共需要" << way.size() << "步" << endl;
    for (int i = way.size() - 1; i >= 0; i--)
    {
        way.at(i).show();
    }
    show(GOAL);
}
#include<iostream>
#include"Deep.h"
using namespace std;
void loop(deque<Situation>& open, deque<Situation>& closed, int range);
int main() {
    string original = "283164705";
    Situation first;
    deque<Situation> open, closed;
    int range = 10;
    first.code = original;
    first.deep = 0;
    open.push_back(first);
    loop(open, closed, range);
    return 0;
}
void loop(deque<Situation>& open, deque<Situation>& closed, int range) {
    Situation a;
    int i = 0;
    while (!open.empty()) {
        cout << i++ << endl;
        if (open.front().code == GOAL) {
            cout << "成功：" << endl;
            a.showAnswer(closed);
            return;
        }
        if (open.empty()) {
            cout << "失败" << endl;
            return;
        }
        closed.push_back(open.front());
        open.pop_front();
        if (closed.back().deep == range) {
            continue;
        }
        else {
            Situation son1 = closed.back().up();
            Situation son2 = closed.back().down();
            Situation son3 = closed.back().left();
            Situation son4 = closed.back().right();
            if (!son1.code.empty()) {
                if (!son1.isInOpen(open) && !son1.isInClosed(closed)) {
                    son1.father = closed.back().code;
                    open.push_front(son1);
                }
            }
            if (!son2.code.empty()) {
                if (!son2.isInOpen(open) && !son2.isInClosed(closed)) {
                    son2.father = closed.back().code;
                    open.push_front(son2);
                }
            }
            if (!son3.code.empty()) {
                if (!son3.isInOpen(open) && !son3.isInClosed(closed)) {
                    son3.father = closed.back().code;
                    open.push_front(son3);
                }
            }
            if (!son4.code.empty()) {
                if (!son4.isInOpen(open) && !son4.isInClosed(closed)) {
                    son4.father = closed.back().code;
                    open.push_front(son4);
                }

            }
            if (son1.isGoal()) {
                cout << "后继节点中有目标节点：" << endl;
                son1.showAnswer(closed);
                break;
            }
            if (son2.isGoal()) {
                cout << "后继节点中有目标节点：" << endl;
                son2.showAnswer(closed);
                break;
            }
            if (son3.isGoal()) {
                cout << "后继节点中有目标节点：" << endl;
                son3.showAnswer(closed);
                break;
            }
            if (son4.isGoal()) {
                cout << "后继节点中有目标节点：" << endl;
                son4.showAnswer(closed);
                break;
            }
        }
    }
}
