The KNearestNeighbors algorithim is fairly simple, you find the euclidiean distance by subtracting the first iterated row by the target
we want to find closest. After that we square the number and find the square root to avoid negative numbers. In the simplest sense we are,
subtracting every row by the target to find which row is closest distance to the target.

{\% highlight python linenos \%}  
	  def euclidiean_distance(row1, row2):
	      distance = 0.0
	      for i in range(len(row1)-1):
		  distance += (row1[i] - row2[i])**2
	      return sqrt(distance)
{\% endhighlight \%}


To get the neighbors it's a bit tricker. You need to append the distances to a list so they're stored in memory
then you sort the distances into a tuple. Finally, you create another list and iterate the distances based on how many 
number of neighbors you want returned.

	def get_neighbors(train, test_row, num_neighbors):
	      distances = list()
	      for train_row in train:
		  dist = euclidiean_distance(test_row, train_row)
		  distances.append((train_row, dist))         
	      distances.sort(key=lambda tup: tup[1])
	      neighbors = list()
	      for i in range(num_neighbors):
		  neighbors.append(distances[i][0])
	      return neighbors

Then finally we can get the neighbors of a point in the dataset by selecting how many neighbors we want:
  
	  dataset = [[2.7810836,2.550537003,0],
		[1.465489372,2.362125076,0],
		[3.396561688,4.400293529,0],
		[1.38807019,1.850220317,0],
		[3.06407232,3.005305973,0],
		[7.627531214,2.759262235,1],
		[5.332441248,2.088626775,1],
		[6.922596716,1.77106367,1],
		[8.675418651,-0.242068655,1],
		[7.673756466,3.508563011,1]]

	  neighbors = get_neighbors(dataset, dataset[0], 3)

	  for neighbor in neighbors:
	      print(neighbor)

	 We get an output of:
	  [4.6, 3.1, 1.5, 0.2, 2]
	  [4.6, 3.2, 1.4, 0.2, 2]
	  [4.7, 3.2, 1.6, 0.2, 2]

  
