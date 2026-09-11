# BFS-va-DFS-bilan-graf
Python’da qo'shnichilik ro'yxatiga asoslangan Graph klassi, hamda BFS va DFS algoritmlarining to'liq amalga oshirilishi:

Python
from collections import deque

class Graph:
    def __init__(self):
        # Qo'shnichilik ro'yxatini lug'at (dict) sifatida saqlaymiz
        self.adj_list = {}

    def add_vertex(self, v):
        """Grafga yangi uch (vertex) qo'shish"""
        if v not in self.adj_list:
            self.adj_list[v] = []

    def add_edge(self, v1, v2):
        """Yo'naltirilmagan graf uchun ikki tomonlama qirra (edge) qo'shish"""
        self.add_vertex(v1)
        self.add_vertex(v2)
        
        # Ikki tomonlama bog'lash
        self.adj_list[v1].append(v2)
        self.adj_list[v2].append(v1)

    def bfs(self, start):
        """Navbat (deque) yordamida BFS algoritmi — O(V + E)"""
        if start not in self.adj_list:
            return []

        visited = set([start])
        queue = deque([start])
        order = []

        while queue:
            vertex = queue.popleft()
            order.append(vertex)

            for neighbor in self.adj_list[vertex]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)

        return order

    def dfs(self, start):
        """Rekursiya yordamida DFS algoritmi — O(V + E)"""
        if start not in self.adj_list:
            return []

        visited = set()
        order = []

        def _dfs(vertex):
            visited.add(vertex)
            order.append(vertex)

            for neighbor in self.adj_list[vertex]:
                if neighbor not in visited:
                    _dfs(neighbor)

        _dfs(start)
        return order
Sinov kodi (Sikl va ajratilgan uch bilan)
Python
# Graf obyektini yaratish
g = Graph()

# Qirralarni qo'shish (Sikli bor komponenta: A-B, B-C, C-A)
g.add_edge('A', 'B')
g.add_edge('B', 'C')
g.add_edge('C', 'A')
g.add_edge('C', 'D')

# Ajratilgan (hech qanday qirrasi yo'q) uchni qo'shish
g.add_vertex('X')

print("Qo'shnichilik ro'yxati:", g.adj_list)
print("-" * 40)

# Sikli bor grafda sinov ('A' dan boshlab)
print("BFS ('A' dan):", g.bfs('A'))  # Kutilgan: ['A', 'B', 'C', 'D']
print("DFS ('A' dan):", g.dfs('A'))  # Kutilgan: ['A', 'B', 'C', 'D']

# Ajratilgan vertex'da sinov
print("\nBFS ('X' dan):", g.bfs('X'))  # Kutilgan: ['X']
print("DFS ('X' dan):", g.dfs('X'))  # Kutilgan: ['X']
Algoritm xususiyatlari
Xotira va Vaqt murakkabligi: Har ikkala algoritm ham har bir uch (V) va qirrani (E) faqat bir marta ko'rib chiqadi, shuning uchun O(V+E) vaqt talab qiladi.

Cheksiz sikldan himoya: visited to'plami (set) orqali avval tashrif buyurilgan uchlar qayta ishlanmaydi.

Keyingi qadamlar uchun quyidagi yo'nalishlardan birini tanlashingiz mumkin:

Navbat o'rniga Stack yordamida iterativ DFS yozish

Grafda sikl bor-yo'qligini aniqlash funksiyasini qo'shish
