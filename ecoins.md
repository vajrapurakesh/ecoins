# ecoins

import java.io.*;

class Coin {
	private int x, y;
	
	Coin(int x, int y) {
		this.x = x;
		this.y = y;
	}
	public int getX() {
		return x;
	}
	public int getY() {
		return y;
	}
}

public class Rakesh {
	public static Problem[] readInput() {
		try {
			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
			int n = Integer.parseInt(br.readLine());
			Problem[] problems = new Problem[n];
			
			String line;
			String[] lineComponents;
			
			int m, s;
			Coin[] coinTypes;
			
			for (int i = 0; i < n; i++) {
				
				
				line = br.readLine();
				lineComponents = line.split(" ");
				m = Integer.parseInt(lineComponents[0]);
				s = Integer.parseInt(lineComponents[1]);
				coinTypes = new Coin[m];
				
				for (int j = 0; j < m; j++) {
					lineComponents = br.readLine().split(" ");
					coinTypes[j] = new Coin(Integer.parseInt(lineComponents[0]), Integer.parseInt(lineComponents[1]));
				}
				problems[i] = new Problem(m, s, coinTypes);
				
				
				br.readLine();
			}
			br.close();
			return problems;
		} 
		catch (IOException e) {
			e.printStackTrace();
			System.exit(0);
		}
		return null;
	}
	
	public static void main(String[] args) {
		Problem[] problems = readInput();
		int[] results = new int[problems.length];
		for (int i = 0; i < problems.length; i++) {
			results[i] = problems[i].solve();
		}
		PrintWriter writer = new PrintWriter(new BufferedOutputStream(System.out));
		for (int i = 0; i < results.length; i++) {
			if (results[i] != Integer.MAX_VALUE) {
				writer.println(results[i]);
			}
			else writer.println("not possible");
		}
		writer.flush();
		writer.close();
	}
}

 class Problem {
	
	private int m;
	
	
	private int s;
	
	
	private Coin[] coinTypes;
	
	Problem(int m, int s, Coin[] coinTypes) {
		this.m = m;
		this.s = s;
		this.coinTypes = coinTypes;
	}
	
	public int solve() {
		int border = s + 1;
		int jBorder = border;
		
		int smallestNrCoins = Integer.MAX_VALUE;
		int[][] shortestPath = new int[border][border];
		
		for (int i = 0; i < border; i++) {
			for (int j = 0; j < border; j++) {
				shortestPath[i][j] = Integer.MAX_VALUE;
			}
		}
		
		shortestPath[0][0] = 0;
		int iPrev, jPrev;
		
		int ssSquare = s*s;
		int eeModSquare;
		
		for (int i = 0; i < border; i++) {
			for (int j = 0; j < jBorder; j++) {
				eeModSquare = i*i + j*j;
				for (int t = 0; t < m; t++) {
					iPrev = i - coinTypes[t].getX();
					jPrev = j - coinTypes[t].getY();
					
					if (iPrev >= 0 && jPrev >= 0) {
						int newPathLength = shortestPath[iPrev][jPrev] + 1;
						if (shortestPath[iPrev][jPrev] != Integer.MAX_VALUE && newPathLength < shortestPath[i][j]) {
							shortestPath[i][j] = newPathLength;
							if (eeModSquare == ssSquare && shortestPath[i][j] < smallestNrCoins) {
								smallestNrCoins = shortestPath[i][j];
								jBorder = j;
							}
							else if (eeModSquare > ssSquare) {
								jBorder = j;
							}
						}
					}
				}
			}
		}
		return smallestNrCoins;
	}
}
