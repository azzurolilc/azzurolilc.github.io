#Random Data Sampling Methods

When I was asked a Reservoir Sampling question during a technical interview, so here is the lesson~ 
The problem is interesting, and sampling methods are fun, I have never done it and this would be good practice for me too. So, some background info and an interesting paper, Sampling Algorithms for Evolving Datasets.

Bernoulli sampling: Poisson sampling with equal first-order inclusion probabilities. Meaning that all elements have equal probability p of being included in sample. Sample size n is random.

Min-wise sampling: Haven't been able to find much resource on the stuff. Will keep updateing.

Here I'm gonna focus on the implementation of --

##Reservoir Sampling

###Java 7:

```java
public static int[] Rs7(int size, Iterable<Integer> data){
        int count = 0;
        int [] arr = new int[size];

        for ( int element: data){
            if (count < size)
                arr[count]=element;
            else{
                Data.random.nextInt(count);
                if (count < size)
                    arr[count] = element;
            } 
            count++;
        }

        return arr;
    }
```

###Java 8(Scala would be fun too~):

```java
    public static int[] Rs8(int size, Iterable<Integer> data) {
        int [] arr = new int[size];
        StreamSupport.stream(data.spliterator(), false)
                .filter(datum -> Data.random.nextDouble()<=size)
                .mapToInt(datum ->Integer.valueOf(datum))
                .toArray();

        return arr;
    }
```

###Thinking about big data and distributed computing:

![](../resources/images/rs.png "Distributed Reservoir Sampling")


Random sampling! This is all I got. Anyways..

(Just hoping that I would not keep discovering these inspiring, bravo problems through interviews..)


[🍀azzurolilc@Twitter](https://twitter.com/azzurolilcz)

The ‘Greenest’ Data Engineer(as of May 2016)>.<