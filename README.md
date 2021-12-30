# OS-Project
Page Replacement Algorithms
#include<iostream>
#include<queue>
#include<unordered_set>
#include<unordered_map>
using namespace std;

const int MaxNum = 100005;
int nunberOfFrames;
int frameCapacity;
int stringArray[MaxNum];
int frameArray[MaxNum];

void FIFO()
{
    queue<int> qList;
    int pageFaults = 0;

    for (int i = 0; i < nunberOfFrames; i++)
    {
        if (frameArray[stringArray[i]] == 1)
        {
            cout << " page " << stringArray[i] << " no page fault" <<endl;
        }
        else
        {
            qList.push(stringArray[i]);
            frameArray[stringArray[i]] = 1;
            if (qList.size() > frameCapacity)
            {
                int x = qList.front();
                frameArray[x] = false;
                qList.pop();
            }
            pageFaults++;
            cout << "  page " << stringArray[i] << " caused a page fault"<<endl;
        }

    }
    cout << "Total number of Page Faults: " << pageFaults;
    return;
}
void Optimal()
{
    vector<int> frames;
    int pageFaults = 0;
    for (int i = 0; i < nunberOfFrames; i++)
    {
        int k;
        for (k = 0; k < frames.size(); k++)
            if (frames[k] == stringArray[i])
                break;
        if (k == frames.size())
        {
            if (frames.size() < frameCapacity)
                frames.push_back(stringArray[i]);

            else
            {
                int ind = i + 1;
                int fres = -1, farind = ind;
                for (int l = 0; l < frames.size(); l++)
                {
                    int j;
                    for (j = ind; j < nunberOfFrames; j++)
                    {
                        if (frames[l] == stringArray[j])
                        {
                            if (j > farind)
                            {
                                farind = j;
                                fres = l;
                            }
                            break;
                        }
                    }
                    if (j == nunberOfFrames)
                    {
                        fres = l;
                        break;
                    }
                }
                frames[fres] = stringArray[i];
            }
            pageFaults++;
            cout << " page:" << stringArray[i] << " caused a page fault" << endl;
        }
        else
        {
            cout << "  page " << stringArray[i] << " no page fault" << endl;
        }
    }
    cout << "Sum of Page Faults: " << pageFaults;
}
void LRU()
{
    unordered_set<int> set;
    unordered_map<int, int> indes;

    int pageFaults = 0;
    for (int i = 0; i < nunberOfFrames; i++)
    {

        if (set.find(stringArray[i]) != set.end())
        {
            cout << "Page:  " << stringArray[i] << " no page fault" << endl;;
        }
        else
        {
            if (set.size() < frameCapacity)
            {
                set.insert(stringArray[i]);
                pageFaults++;
            }
            else
            {
                int LRU = INT_MAX, value;
                for (auto itr : set)
                {
                    if (indes[itr] < LRU)
                    {
                        LRU = indes[itr];
                        value = itr;
                    }
                }
                set.erase(value);
                set.insert(stringArray[i]);
                pageFaults++;
            }
            cout << " page " << stringArray[i] << " caused a page fault" << endl;

        }
        indes[stringArray[i]] = i;
    }

    cout << "Total Page Faults: " << pageFaults;
}

int main()
{
    cout << "Frame size : ";
    cin >> frameCapacity;

    cout << "string you want to enter: ";
    cin >> nunberOfFrames;

    cout << "String Stream:" << endl;;
    for (int i = 0; i < nunberOfFrames; i++)
        cin >> stringArray[i];
    while (true)
    {
        cout << "SIMULATION OF FIFO || LRU || OPTIMAL ALGORITHMS" << endl;

        cout << "1 ) First In First Out Algorithm." << endl;
        cout << "2 ) Least Recently Used Algorithm" << endl;
        cout << "3 ) Optimal Algorithm." << endl;
        cout << "4 ) Quit." << endl;
        int choiceoice;
        cin >> choice;
        if (choice == 1)
        {

            FIFO();
        }
        else if (choice == 2)
        {
            LRU();
        }
        else if (choice == 3)
        {
            Optimal();
        }
        else
            break;

    }



    return 0;
}
