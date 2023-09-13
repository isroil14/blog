---
title: "Linked List In Go"
date: 2023-09-11
draft: false
---

***

[Wiki](https://en.wikipedia.org/wiki/Linked_list)

In my design, indexing starts with 1, not 0. Index 1 refers to the first node while index -1 refers to the last node.

Lets assume our list has 4 nodes already. While you can add a new node with an index 5 (that is going to be the last node), you cannot retrieve a node with an index 5. If you try to add a new node with an index 4 though, then that new node is going to be inserted just before the last node.

In other words:

Insert asks for the future index of a node while Get and Remove asks for the present index of a node.

List and Node structs

    package main

    import "fmt"

    type List struct {
        head   *Node
        length int
    }

    type Node struct {
        data string
        next *Node
    }

    func New() *List {
        return &List{}
    }

Insert method on the List

    func (l *List) Insert(index int, data string) {
        if index == 0 {
            panic("invalid index number 0")
        }

        if index > l.length+1 || index < -l.length-1 {
            panic("index out of boundary")
        }

        if index == 1 {
            l.head = &Node{data, l.head}
            l.length++

            return
        }

        if index > 1 {
            temp := l.head

            for index > 2 {
                temp = temp.next
                index--
            }

            temp.next = &Node{data, temp.next}
            l.length++

            return
        } else {
            l.Insert(l.length+index+2, data)
        }
    }
Remove method on the List

    func (l *List) Remove(index int) {
        if index == 0 {
            panic("invalid index number 0")
        }

        if index > l.length || index < -l.length {
            panic("index out of boundary")
        }

        if index == 1 {
            l.head = l.head.next
            l.length--

            return
        }
        if index > 1 {
            temp := l.head

            for index > 2 {
                temp = temp.next
                index--
            }

            temp.next = temp.next.next
            l.length--

            return
        } else {
            l.Remove(l.length + index + 1)
        }
    }

Get method on the List

    func (l *List) Get(index int) string {
        if index == 0 {
            panic("invalid index number 0")
        }

        if index > l.length || index < -l.length {
            panic("index out of boundary")
        }

        if index > 0 {
            temp := l.head

            for index > 1 {
                temp = temp.next
                index--
            }

            return temp.data
        } else {
            return l.Get(l.length + index + 1)
        }
    }

Main "main" function

    func main() {
        names := New()

        names.Insert(1, "Benhur")
        names.Insert(1, "Joseph")
        names.Insert(2, "Maxwell")
        names.Insert(4, "Tessa")
        names.Insert(1, "Bernard")
        names.Insert(3, "Alisson")

        // Benhur
        // Joseph > Benhur
        // Joseph > Maxwell > Benhur
        // Joseph > Maxwell > Benhur > Tessa
        // Bernard > Joseph > Maxwell > Benhur > Tessa
        // Bernard > Joseph > Alisson > Maxwell > Benhur > Tessa

        fmt.Printf("\nFrom the first to the last\n\n")
        for index := 1; index <= names.length; index++ {
            fmt.Printf("[%d]\t%s\n", index, names.Get(index))
        }

        fmt.Printf("\nFrom the last to the first\n\n")
        for index := -1; index >= -names.length; index-- {
            fmt.Printf("[%d]\t%s\n", names.length + index + 1, names.Get(index))
        }

        names.Remove(-2)
        names.Remove(3)

        // Bernard > Joseph > Alisson > Maxwell > Tessa
        // Bernard > Joseph > Maxwell > Tessa

        fmt.Printf("\nFrom the first to the last\n\n")
        for index := 1; index <= names.length; index++ {
            fmt.Printf("[%d]\t%s\n", index, names.Get(index))
        }

        fmt.Printf("\nFrom the last to the first\n\n")
        for index := -1; index >= -names.length; index-- {
            fmt.Printf("[%d]\t%s\n", names.length + index + 1, names.Get(index))
        }
    }

Output

    From the first to the last

    [1]     Bernard
    [2]     Joseph
    [3]     Alisson
    [4]     Maxwell
    [5]     Benhur
    [6]     Tessa

    From the last to the first

    [6]     Tessa
    [5]     Benhur
    [4]     Maxwell
    [3]     Alisson
    [2]     Joseph
    [1]     Bernard

    From the first to the last

    [1]     Bernard
    [2]     Joseph
    [3]     Maxwell
    [4]     Tessa

    From the last to the first

    [4]     Tessa
    [3]     Maxwell
    [2]     Joseph
    [1]     Bernard

Thank you so much for reading.

Take proper care of yourself. You are your highest priority :)