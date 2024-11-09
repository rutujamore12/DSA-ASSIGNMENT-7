#include <iostream>
using namespace std;

const int V = 5; // Number of vertices (Constant)

struct Edge {
    int src, des, weight;
};

int minKey(int key[], bool inMST[]) {
    int min = 999999;
    int min_index;
    for (int v = 0; v < V; v++) {
        if (!inMST[v] && key[v] < min) {
            min = key[v];
            min_index = v;
        }
    }
    return min_index;
}

void primMST(int graph[V][V]) {
    int parent[V], key[V];
    bool inMST[V] = {false};

    for (int i = 0; i < V; i++) {
        key[i] = 999999;
        parent[i] = -1;
    }
    key[0] = 0;

    for (int count = 0; count < V - 1; count++) {
        int u = minKey(key, inMST);
        inMST[u] = true;

        for (int v = 0; v < V; v++) {
            if (graph[u][v] && !inMST[v] && graph[u][v] < key[v]) {
                parent[v] = u;
                key[v] = graph[u][v];
            }
        }
    }

    cout << "Minimum Spanning Tree (MST) using Prim's algorithm :\nEdge \tWeight\n";
    for (int i = 1; i < V; i++) {
        cout << parent[i] << "-" << i << "\t" << graph[i][parent[i]] << "\n";
    }
}

int find(int parent[], int i) {
    if (parent[i] == -1)
        return i;
    return find(parent, parent[i]);
}

void unionSets(int parent[], int x, int y) {
    int xset = find(parent, x);
    int yset = find(parent, y);
    parent[xset] = yset;
}

void kruskalMST(Edge edges[], int edgeCount) {
    // Sort edges based on weight
    for (int i = 0; i < edgeCount - 1; i++) {
        for (int j = 0; j < edgeCount - i - 1; j++) {
            if (edges[j].weight > edges[j + 1].weight) {
                Edge temp = edges[j];
                edges[j] = edges[j + 1];
                edges[j + 1] = temp;
            }
        }
    }

    int parent[V];
    for (int i = 0; i < V; i++) {
        parent[i] = -1;
    }

    cout << "Minimum Spanning Tree (MST) using Kruskal's algorithm:\nEdge \tWeight\n";
    for (int i = 0; i < edgeCount; i++) {
        int src = edges[i].src;
        int dest = edges[i].des;
        int weight = edges[i].weight;

        if (find(parent, src) != find(parent, dest)) {
            cout << src << "-" << dest << "\t" << weight << "\n";
            unionSets(parent, src, dest);
        }
    }
}

void inputGraph(int graph[V][V]) {
    cout << "Enter the adjacency matrix (0 for no connection): \n";
    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            cin >> graph[i][j];
        }
    }
}

void inputKruskalGraph(Edge edges[], int &edgeCount) {
    cout << "Enter the number of edges: ";
    cin >> edgeCount;
    cout << "Enter edges in the format: src dest weight\n";
    for (int i = 0; i < edgeCount; i++) {
        cin >> edges[i].src >> edges[i].des >> edges[i].weight;
    }
}

int main() {
    int graph[V][V];
    inputGraph(graph);
    primMST(graph);

    Edge edges[V * (V - 1) / 2];
    int edgeCount;
    inputKruskalGraph(edges, edgeCount);
    kruskalMST(edges, edgeCount);

    return 0;
}# DSA-ASSIGNMENT-7
