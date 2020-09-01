# 1. Binary Search Tree

# 2. insert, delete

insert
```java
    // 중복된 데이터는 insert 되면 안되므로 else if 처리.
    private static Node insert(Node t, int x) {
        if(t == null) return new Node(x);
        
        if(t.data < x) t.right = insert(t.right, x);
        else if (t.data > x) t.left = insert(t.left, x);

        return t;
    }
```

delete (delete 는 삭제하고자 하는 원소 노드를 treeSearch 를 통해 찾아서 제거해야 하지만 우선 delete 메소드만 기술)
```java
private static Node delete(Node t, Node r, Node p) {
        if(t.data == r.data) {
            t = deleteNode(t);
        } else if(r.data == p.left.data) {
            p.left = deleteNode(r);
        } else {
            p.right = deleteNode(r);
        }

        return t;
    }

    private static Node deleteNode(Node r) {
        if(r.left == null && r.right == null) {
            return null;
        } else if(r.left == null && r.right != null) {
            return r.right;
        } else if(r.left != null && r.right == null) {
            return r.left;
        } else {
            Node parent = null;
            Node s = r.right;
            while(s.left != null) {
                parent = s;
                s = s.left;
            }

            r.data = s.data;
            if(r.right.data == s.data) {
                r.right = s.right;
            } else {
                parent.left = s.right;
            }

            return r;
        }
    }
```

# 3. Binary Search Tree : Lowest Common Ancestor

- [Hacker rank 문제](hackerrank.com/challenges/binary-search-tree-lowest-common-ancestor/problem)

```java
public class LowestCommonAncestor {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int t = scan.nextInt();
        Node root = null;
        while(t-- > 0) {
            int data = scan.nextInt();
            root = insert(root, data);
        }
        int v1 = scan.nextInt();
        int v2 = scan.nextInt();
        scan.close();
        Node ans = lca(root,v1,v2);
        System.out.println(ans.data);
    }

    public static Node lca(Node root, int v1, int v2) {
        // Write your code here.
        HashMap<Integer, Integer> map = new HashMap();

        treeSearch(root, v1, map, 0);
        treeSearch(root, v2, map, 1);

        int val = map.entrySet().stream().max(Comparator.comparingInt(Map.Entry::getValue)).get().getKey();

        return new Node(val);
    }

    public static Node treeSearch(Node t, int x, HashMap<Integer, Integer> map, int i) {
        if(!map.containsKey(t.data)) map.put(t.data, 0);
        else map.put(t.data, map.get(t.data) + (i++));
        if(t.data == x) return new Node(x);

        if(x < t.data) {
            return treeSearch(t.left, x, map, i);
        }

        return treeSearch(t.right, x, map, i);
    }

    public static Node insert(Node root, int data) {
        if(root == null) {
            return new Node(data);
        } else {
            Node cur;
            if(data <= root.data) {
                cur = insert(root.left, data);
                root.left = cur;
            } else {
                cur = insert(root.right, data);
                root.right = cur;
            }
            return root;
        }
    }

    private static class Node {
        Node left;
        Node right;
        int data;

        Node(int data) {
            this.data = data;
            left = null;
            right = null;
        }
    }
}
```
