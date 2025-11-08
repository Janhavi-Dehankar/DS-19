# DS-19
DS code
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX 100

int adj[MAX][MAX], n, time;

bool visited[MAX];
int disc[MAX], low[MAX], parent[MAX];

bool APUtil(int u) {
    int children = 0;
    visited[u] = true;
    disc[u] = low[u] = ++time;

    for (int v = 0; v < n; v++) {
        if (adj[u][v]) {
            if (!visited[v]) {
                children++;
                parent[v] = u;
                if (APUtil(v)) return true;
                low[u] = low[u] < low[v] ? low[u] : low[v];
                if ((parent[u] == -1 && children > 1) || (parent[u] != -1 && low[v] >= disc[u]))
                    return true; 
            } else if (v != parent[u])
                low[u] = low[u] < disc[v] ? low[u] : disc[v];
        }
    }
    return false;
}

bool isBiconnected() {
    for (int i = 0; i < n; i++) visited[i] = false, parent[i] = -1;
    time = 0;
    if (APUtil(0)) return false;
    for (int i = 0; i < n; i++)
        if (!visited[i]) return false; 
    return true;
}

int main() {
    int edges, u, v;
    printf("Enter number of vertices: ");
    scanf("%d", &n);
    printf("Enter number of edges: ");
    scanf("%d", &edges);
    for (int i = 0; i < edges; i++) {
        printf("Enter edge (u v): ");
        scanf("%d %d", &u, &v);
        adj[u][v] = adj[v][u] = 1;
    }
    if (isBiconnected()) printf("Graph is biconnected\n");
    else printf("Graph is not biconnected\n");
    return 0;
}
