#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef struct Node
{
    bool isNegative;
    int Data;
    struct Node *Next;
    struct Node *Previous;
} Node;

typedef Node *DoublyList;
typedef Node *DoublyNode;
typedef Node *PtrToDoublyNode;

/*________________________________________________________*/

void CreatDoublyList(DoublyList *list);
int IsEmpty(DoublyList list);
int IsLast(DoublyNode node);
DoublyNode FindNode(int x, DoublyList list);
void DeleteNode(int x, DoublyList list);
void Insert(int x, DoublyList list, DoublyNode prevNode);
void PrintList(DoublyList list);
void MakeEmpty(DoublyList *list);
int Size(DoublyList list);
void DisposeList(DoublyList *list);
DoublyList CopyList(DoublyList list, DoublyList copiedList);
int CompareLists(DoublyList list1, DoublyList list2);
void RemoveLeftZeros(DoublyList* list);


/*________________________________________________________*/

void CreatDoublyList(DoublyList *list)
{

    // if(*list != NULL)
    // {
    //     DisposeList(&(*list));
    // }
    (*list) = (DoublyList)malloc(sizeof(Node));

    if (*list == NULL)
    {
        printf("Out Of Memory!");
        exit(0);
    } // end if()
    (*list)->Next = NULL;
    (*list)->Previous = NULL;
    (*list)->isNegative = false;

} // end of MakeEmpty()

/*________________________________________________________*/

int IsEmpty(DoublyList list)
{
    return list->Next == NULL;
} // end of IsEmpty()

/*________________________________________________________*/

int IsLast(DoublyNode node)
{
    return node->Next == NULL;
} // end of IsLast()

/*________________________________________________________*/

DoublyNode FindNode(int x, DoublyList list)
{
    DoublyNode tmp = list->Next;
    while (tmp != NULL && tmp->Data != x)
    {
        tmp = tmp->Next;
    } // end of while()
    return tmp;
} // end of Find()

/*________________________________________________________*/

void DeleteNode(int x, DoublyList list)
{
    DoublyNode tmp1 = list->Next;
    while (tmp1 != NULL && tmp1->Data != x)
    {
        tmp1 = tmp1->Next;
    }
    if (tmp1 != NULL)
    {
        if (tmp1->Previous != NULL)
        {
            tmp1->Previous->Next = tmp1->Next;
        }
        if (tmp1->Next != NULL)
        {
            tmp1->Next->Previous = tmp1->Previous;
        }
        free(tmp1);
    }
} // end of Delete()

/*________________________________________________________*/

void Insert(int x, DoublyList list, DoublyNode prevNode)
{
    DoublyNode node = malloc(sizeof(Node));
    if (node == NULL)
    {
        printf("Memory Is Full...\n");
        return;
    }
    else
    {
        node->Data = x;
        node->Next = prevNode->Next;
        node->Previous = prevNode;
        prevNode->Next = node;
        if (node->Next != NULL)
        {
            node->Next->Previous = node;
        } // end if()
    }     // end else
}

/*________________________________________________________*/

void PrintList(DoublyList list)
{
    DoublyNode tmp = list;
    if (list != NULL)
    {
        if (tmp->Next == NULL)
        {
            printf("Empty List!\n");
        }
        else
        {
            while (tmp->Next->Data == 0 && tmp->Next->Next != NULL)
            {
                tmp->Next = tmp->Next->Next;
            }
            if (list->isNegative == true)
            {
                printf("-");
            } // end if()
            while (tmp->Next != NULL)
            {
                printf("%d", tmp->Next->Data);
                tmp = tmp->Next;
            }
            printf("\n");
        }
    } // end BIG if()
    else
    {
        printf("Null");
    }
} // end of PrintList()

/*________________________________________________________*/

void MakeEmpty(DoublyList *list)
{
    if ((*list)->Next != NULL)
    {
        DoublyNode tmp1 = (*list)->Next;
        DoublyNode tmp2 = tmp1->Next;
        while (tmp2 != NULL)
        {
            free(tmp1);
            tmp1 = tmp2;
            tmp2 = tmp2->Next;
        } // end while()
        free(tmp1);
        (*list)->Next = NULL;
        (*list)->Previous = NULL;
    }

} // end DeleteList()

/*________________________________________________________*/

int Size(DoublyList list)
{
    int size = 0;
    DoublyNode tmp = list;
    while (tmp->Next != NULL)
    {
        size++;
        tmp = tmp->Next;
    } // end while()
    return size;
} // end Size()

/*__________________________________________________________________________*/

DoublyNode lastDoublyNode(DoublyList list)
{
    DoublyNode tmp = list;
    while (!IsLast(tmp))
    {
        tmp = tmp->Next;
    } // end while()
    return tmp;
} // end lastDoublyNode()

/*__________________________________________________________________________*/

void InsertLast(int x, DoublyList list)
{
    DoublyNode tmp = lastDoublyNode(list);
    DoublyNode node = malloc(sizeof(Node));
    if (node == NULL)
    {
        printf("Memory Is Full...\n");
        return;
    }
    else
    {
        Insert(x, list, tmp);
    } // end else
} // end of InsertLast()

/*__________________________________________________________________________*/

void DisposeList(DoublyList *list)
{
    PtrToDoublyNode tmp1 = *list;
    PtrToDoublyNode tmp2 = tmp1->Next;

    while (tmp2 != NULL)
    {
        free(tmp1);
        tmp1 = tmp2;
        tmp2 = tmp2->Next;
    } // end while()
    free(tmp1);
    *list = NULL;
} // end DisposeList()

/*__________________________________________________________________________*/

DoublyList CopyList(DoublyList list, DoublyList copiedList)
{
    DoublyList newList = NULL;
    CreatDoublyList(&newList);
    newList->isNegative = copiedList->isNegative;

    DoublyNode tmp1 = copiedList->Next;
    DoublyNode lastNode = newList;

    if (copiedList == NULL)
    {
        printf("Null Pointer");
    }
    else
    {
        while (tmp1 != NULL)
        {
            Insert(tmp1->Data, newList, lastNode);
            lastNode = lastNode->Next;
            tmp1 = tmp1->Next;
        }
    }

    return newList;
} // end of CopyList()

int CompareLists(DoublyList list1, DoublyList list2)
{
    // if(list1->isNegative == true && list2->isNegative == false)
    // {
    //     return -1;
    // } // end if()
    // else if(list1->isNegative == false && list2->isNegative == true)
    // {
    //     return 1;
    // } // end else if()
    
    long size1 = Size(list1);
    long size2 = Size(list2);

    PtrToDoublyNode tmp1 = list1->Next;
    PtrToDoublyNode tmp2 = list2->Next;

    if (size1 > size2)
    {
        return 1;
    } // end if()
    else if (size2 > size1)
    {
        return -1;
    } // end if()
    else
    {
        for (int i = 0; i < size1; i++)
        {
            if (tmp1->Data > tmp2->Data)
            {
                return 1;
            } // end if()
            else if (tmp1->Data < tmp2->Data)
            {
                return -1;
            } // end else if()
            tmp1 = tmp1->Next;
            tmp2 = tmp2->Next;
        } // end for()
         return 0;
    } // end else   
} // end CompareLists()

void RemoveLeftZeros(DoublyList* list) {
    PtrToDoublyNode tmp = (*list)->Next;
    while (tmp->Next != NULL && tmp->Data == 0) {
        PtrToDoublyNode nextNode = tmp->Next;
        free(tmp);
        tmp = nextNode;
    }
    (*list)->Next = tmp;
} // end of RemoveLeftZeros()
