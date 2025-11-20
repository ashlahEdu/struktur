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


import java.util.Stack;

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

    // Directed
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

    // DFS dari satu vertex
    private void dfsFrom(int start) {
        Stack<Integer> stack = new Stack<>();
        vertexList[start].wasVisited = true;
        stack.push(start);

        while (!stack.isEmpty()) {
            int v = stack.peek();
            int next = getAdjUnvisitedVertex(v);

            if (next == -1) {
                stack.pop();
            } else {
                vertexList[next].wasVisited = true;
                stack.push(next);
            }
        }
    }

    // Connectivity Table
    public void connectivityTable() {
        int[][] table = new int[nVerts][nVerts];

        for (int start = 0; start < nVerts; start++) {
            resetFlags();
            dfsFrom(start);

            for (int i = 0; i < nVerts; i++)
                table[start][i] = vertexList[i].wasVisited ? 1 : 0;
        }

        // Tampilkan tabel
        System.out.println("Connectivity Table:");
        for (int i = 0; i < nVerts; i++) {
            System.out.print(vertexList[i].label + " : ");
            for (int j = 0; j < nVerts; j++)
                System.out.print(table[i][j] + " ");
            System.out.println();
        }

        resetFlags();
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

