class DSU:
    def __init__(self, n):
        self.p = list(range(n)) #parrent array
        self.r = [0]*n #rank array

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x
        
    def union(self, a, b):
        
        ra, rb = self.find(a), self.find(b)
        if ra == rb: return
        if self.r[ra] < self.r[rb]:
            self.p[ra] = rb
        elif self.r[ra] > self.r[rb]:
            self.p[rb] = ra
        else:
            self.p[rb] = ra
            self.r[ra] += 1
        print(self.p)

def can_pass(obstacles, y_bottom=1.0, y_top=5.0, clearance=0.2):
    # clearance is for the radius of the robot
    # obstacles: list of (x, y, r)
    # inflate radii and shrink walls by clearance
    yb, yt = y_bottom + clearance, y_top - clearance
    obs = [(x, y, r + clearance) for (x, y, r) in obstacles]
    print(obs)

    n = len(obs)
    TOP, BOTTOM = n, n+1 #top and bottom walls as obstacles as well
    dsu = DSU(n + 2)

    # circle-wall unions
    for i, (x, y, r) in enumerate(obs):
        if y - r <= yb: dsu.union(i, BOTTOM)
        if y + r >= yt: dsu.union(i, TOP)

    # circle-circle unions
    for i in range(n):
        xi, yi, ri = obs[i]
        for j in range(i+1, n):
            xj, yj, rj = obs[j]
            dx, dy = xi - xj, yi - yj
            if dx*dx + dy*dy <= (ri + rj)*(ri + rj):
                dsu.union(i, j)

    # If TOP and BOTTOM connect, barrier exists → cannot pass
    return dsu.find(TOP) != dsu.find(BOTTOM)

# Example:
obstacles = [(2, 2.0, 0.8), (4, 3.0, 1.2), (6, 4.2, 0.7)]
print( obstacles)
print("Passable?" , can_pass(obstacles, clearance=0.2))
