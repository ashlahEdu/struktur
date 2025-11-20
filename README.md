# struktur
1. 
public class Vertex {
    public char label;
    public boolean wasVisited;

    public Vertex(char label) {
        this.label = label;
        this.wasVisited = false;
    }
}

public class Graph {
    private final int MAX_VERTS = 20;
    private Vertex[] vertexList;
    private int[][] adjMat;
    private int nVerts;

    public Graph() {
        vertexList = new Vertex[MAX_VERTS];
        adjMat = new int[MAX_VERTS][MAX_VERTS];
        nVerts = 0;

        for (int i = 0; i < MAX_VERTS; i++)
            for (int j = 0; j < MAX_VERTS; j++)
                adjMat[i][j] = 0;
    }

    public void addVertex(char label) {
        vertexList[nVerts++] = new Vertex(label);
    }

    // Directed graph → hanya 1 arah
    public void addEdge(int start, int end) {
        adjMat[start][end] = 1;
    }

    public void displayEdges() {
        System.out.println("Edges:");
        for (int i = 0; i < nVerts; i++) {
            for (int j = 0; j < nVerts; j++) {
                if (adjMat[i][j] == 1) {
                    System.out.println(vertexList[i].label + " -> " + vertexList[j].label);
                }
            }
        }
    }
}

public class GraphApp1 {
    public static void main(String[] args) {

        Graph g = new Graph();

        // index: 0=A, 1=B, 2=C, 3=D, 4=E
        g.addVertex('A');
        g.addVertex('B');
        g.addVertex('C');
        g.addVertex('D');
        g.addVertex('E');

        // edges sesuai gambar 10.8
        g.addEdge(0, 1); // A -> B
        g.addEdge(0, 2); // A -> C
        g.addEdge(1, 4); // B -> E
        g.addEdge(3, 4); // D -> E
        g.addEdge(4, 2); // E -> C (panah balik)

        g.displayEdges();
    }
}

2.
public class Vertex {
    public char label;
    public boolean wasVisited;

    public Vertex(char label) {
        this.label = label;
        this.wasVisited = false;
    }
}

public class StackX {
    private int maxSize;
    private int[] stackArray;
    private int top;

    public StackX(int s) {
        maxSize = s;
        stackArray = new int[s];
        top = -1;
    }

    public void push(int j) {
        stackArray[++top] = j;
    }

    public int pop() {
        return stackArray[top--];
    }

    public int peek() {
        return stackArray[top];
    }

    public boolean isEmpty() {
        return (top == -1);
    }
}

public class Graph {
    private final int MAX_VERTS = 20;
    private Vertex[] vertexList;
    private int[][] adjMat;
    private int nVerts;

    public Graph() {
        vertexList = new Vertex[MAX_VERTS];
        adjMat = new int[MAX_VERTS][MAX_VERTS];
        nVerts = 0;

        for (int i = 0; i < MAX_VERTS; i++)
            for (int j = 0; j < MAX_VERTS; j++)
                adjMat[i][j] = 0;
    }

    public void addVertex(char label) {
        vertexList[nVerts++] = new Vertex(label);
    }

    // directed
    public void addEdge(int start, int end) {
        adjMat[start][end] = 1;
    }

    private int getAdjUnvisitedVertex(int v) {
        for (int j = 0; j < nVerts; j++)
            if (adjMat[v][j] == 1 && !vertexList[j].wasVisited)
                return j;
        return -1;
    }

    private void resetFlags() {
        for (int i = 0; i < nVerts; i++)
            vertexList[i].wasVisited = false;
    }

    private void dfsFrom(int start) {
        StackX stack = new StackX(MAX_VERTS);

        vertexList[start].wasVisited = true;
        stack.push(start);

        while (!stack.isEmpty()) {
            int top = stack.peek();
            int next = getAdjUnvisitedVertex(top);

            if (next == -1) {
                stack.pop();
            } else {
                vertexList[next].wasVisited = true;
                stack.push(next);
            }
        }
    }

    public void connectivityTable() {
        int[][] table = new int[nVerts][nVerts];

        for (int start = 0; start < nVerts; start++) {
            resetFlags();
            dfsFrom(start);

            for (int i = 0; i < nVerts; i++)
                table[start][i] = vertexList[i].wasVisited ? 1 : 0;
        }

        System.out.println("Connectivity Table:");
        for (int i = 0; i < nVerts; i++) {
            System.out.print(vertexList[i].label + " : ");
            for (int j = 0; j < nVerts; j++)
                System.out.print(table[i][j] + " ");
            System.out.println();
        }
    }
}

public class GraphApp2 {
    public static void main(String[] args) {

        Graph g = new Graph();

        // index: 0=A, 1=B, 2=C, 3=D, 4=E
        g.addVertex('A');
        g.addVertex('B');
        g.addVertex('C');
        g.addVertex('D');
        g.addVertex('E');

        g.addEdge(0, 1); // A -> B
        g.addEdge(0, 2); // A -> C
        g.addEdge(1, 4); // B -> E
        g.addEdge(3, 4); // D -> E
        g.addEdge(4, 2); // E -> C

        g.connectivityTable();
    }
}
