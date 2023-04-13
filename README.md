1. Вывести первые 3 элемента массива (Вывести первые 3 элемента массива используя цикл for).

Ответ:

	public static void main(String[] args) {
		int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;

		for ( int i = 0; i < nums.length-4; i++)
		{
			System.out.println(nums[i]);
		}
	}


2. Вывести первую половину массива (Вывести первую первую половину элементов массива при помощи цикла for). Реализация задачи должна работать при любом чётном количестве элементов.

Ответ:

	public static void main(String[] args) {
		int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;
		for ( int i = 0; i < nums.length/2; i++)
		{
			System.out.println(nums[i]);
		}
	}


3. Вывести вторую половину массива (Вывести вторую половину элементов массива при помощи цикла for). Реализация задачи должна работать при любом чётном количестве элементов.

Ответ:


   	public static void main(String[] args) {
   
      int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;
      for ( int i = nums.length/2; i < nums.length;  i++)
      {
         System.out.println(nums[i]);
      }
     }


4. Вывести все элементы кроме первого и последнего.

Ответ:

	public static void main(String[] args) {
		int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;

		for (int i = 1; i < nums.length-1; i++)
		{
			System.out.println(nums[i]);
		}
	}

5. Вывести последние 3 элемента массива (Для массива [7, -3, 9, -11, 18, 99, 2, 11] вывод должен быть таким: 99, 2, 11)

Ответ:

	public static void main(String[] args) {
		int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;

		for (int i = 5; i < nums.length; i++)
		{
			System.out.println(nums[i]);
		}


	}


6. Вывести чётные элементы массива по порядку (Вывести только чётные элементы массива по порядку (каждый второй элемент). Необходимо будет вывести второй, четвёртый, шестой и т.д. элементы. Позиции (индексы) для необходимых элементов: 1, 3, 5...n (постоянное увеличение на 2).

Ответ:

	public static void main(String[] args) {
		int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;

		for (int i = 0; i < nums.length; i++)
			if(i%2==1)
		{
			System.out.println(nums[i]);
		}
	}




7. Найти максимальный и минимальный элементы массива (Необходимо определить максимальный и минимальный элементы в массиве и вывести их).

Ответ:

    public static void main(String[] args) {
      int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};
      Arrays.sort(nums);
      System.out.println("Minimum = " + nums[0]);
      System.out.println("Maximum = " + nums[nums.length - 1]);
    }

