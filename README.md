splay
=====
<b>A polymorphic splay tree implementation in Go(lang)</b>

splay is defined around the <code>SplayTree</code> interface:  

    type SplayTree interface {
        SetRoot(n *Node)
        GetRoot() *Node
        Ord(key1, key2 interface{}) int
    }

Your <code>SplayTree</code> implementation must contain a <code>root *Node</code> where

    type Node struct {
        key    interface{}
        value  interface{}
        parent *Node
        left   *Node
        right  *Node
    }

<code>SetRoot</code> and <code>GetRoot</code> are the getter and setter for root, and <code>Ord</code> is a method for
comparing keys that returns 1 if key1 < key2, 2 if key1 = key2, or 3 if
key1 > key2.

Here's an example assuming we have the splay directory in the same directory as
our project (a very similar example is given in <i>example.go</i>):

    import "./splay"

    type Tree struct {
        root *splay.Node
    }

    func (T *Tree) SetRoot(n *splay.Node) {
        T.root = n
    }

    func (T *Tree) GetRoot() *splay.Node {
        return T.root
    }

    // This Splay Tree will use strings as keys and strings as values
    // It returns 0, 1, or 2 based on the lexical comparison of the key strings
    func (T *Tree) Ord(key1, key2 interface{}) int {
        for i := 0; i < utf8.RuneCountInString(key1.(string)); {
            c1, j := utf8.DecodeRuneInString(key1.(string)[i:])
            c2, _ := utf8.DecodeRuneInString(key2.(string)[i:])
            if c1 < c2 {
                return 0
            } else if c1 > c2 {
                return 2
            }
            i += j
        }
        if len(key1.(string)) < len(key2.(string)) {
            return 0
        } else if len(key1.(string)) == len(key2.(string)) {
            return 1
        } else {
            return 2
        }
    }

    func main() {
        T := new(Tree)

        splay.Insert(T, "hello", "world")
        splay.Insert(T, "foo", "bar")
        splay.Insert(T, "Goku says", "Kamehameha")
        err := splay.Insert(T, "hello")
        fmt.Println(err)

        v := splay.Find("Goku")
        fmt.Println(v)

        splay.Print(T)

        splay.Delete(T, "foo")
        err = splay.Delete(T, "foo")
        fmt.Println(err)
				
        splay.Print(T)
    }

		
<b>Copyright 2013 Nathaniel Travis<br>
Distributed under the GNU General Public License (see gpl.txt)</b>

<i>Note: this was a Golang learning exercise for me, so I may have broken some
style rules.</i>
