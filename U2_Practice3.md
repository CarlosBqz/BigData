# Practice 3: Team 1 Basic Statistics

#### Pearson and Spearman Correlation

- At the beginning of the code the team import the libraries to use matrices, vectors, rows and sessions in Spark.

- After that, the team create an object named Correlation Example to run a program in an easier way.

- Then, the team define a function where they start a spark session and load the Correlation Example that its stored in the Spark installation folder.

- To finalize they apply the Correlation.corr function  to the loaded df, that will show us the Pearson Correlation aswell as the Spearman Correlation.

```scala
import org.apache.spark.ml.linalg.{Matrix, Vectors}
import org.apache.spark.ml.stat.Correlation
import org.apache.spark.sql.Row
import org.apache.spark.sql.SparkSession

object CorrelationExample {

  def main(args: Array[String]): Unit = {
    val spark = SparkSession
      .builder
      .appName("CorrelationExample")
      .getOrCreate()
    import spark.implicits._

    val data = Seq(
      Vectors.sparse(4, Seq((0, 1.0), (3, -2.0))),
      Vectors.dense(4.0, 5.0, 0.0, 3.0),
      Vectors.dense(6.0, 7.0, 0.0, 8.0),
      Vectors.sparse(4, Seq((0, 9.0), (3, 1.0)))
    )

    val df = data.map(Tuple1.apply).toDF("features")
    val Row(coeff1: Matrix) = Correlation.corr(df, "features").head
    println(s"Pearson correlation matrix:\n $coeff1")

    val Row(coeff2: Matrix) = Correlation.corr(df, "features", "spearman").head
    println(s"Spearman correlation matrix:\n $coeff2")

    spark.stop()
  }
}
```

#### Chi Square

- At the beginning of the code the team import the libraries to use Chi Square Tests, vectors and sessions in Spark.

- After that, the team create an object named ChiSquareTest Example to run a program in an easier way.

- Then, the team define a function where they start a spark session and load the Chi Square Example that its stored in the Spark installation folder.

- To finalize, they create a data value as an example of information, creating a data frame using that value in two columns and finalizing with the pValues, the degrees of freedom and the statistics.

```scala
import org.apache.spark.ml.linalg.{Vector, Vectors}
import org.apache.spark.ml.stat.ChiSquareTest
import org.apache.spark.sql.SparkSession

object ChiSquareTestExample {

  def main(args: Array[String]): Unit = {
    val spark = SparkSession
      .builder
      .appName("ChiSquareTestExample")
      .getOrCreate()
    import spark.implicits._

    val data = Seq(
      (0.0, Vectors.dense(0.5, 10.0)),
      (0.0, Vectors.dense(1.5, 20.0)),
      (1.0, Vectors.dense(1.5, 30.0)),
      (0.0, Vectors.dense(3.5, 30.0)),
      (0.0, Vectors.dense(3.5, 40.0)),
      (1.0, Vectors.dense(3.5, 40.0))
    )

    val df = data.toDF("label", "features")
    val chi = ChiSquareTest.test(df, "features", "label").head
    println(s"pValues = ${chi.getAs[Vector](0)}")
    println(s"degreesOfFreedom ${chi.getSeq[Int](1).mkString("[", ",", "]")}")
    println(s"statistics ${chi.getAs[Vector](2)}")

    spark.stop()
  }
}
```

#### Summarizer

- At the beginning of the code the team import the libraries to use vectors, summarizer and sessions in Spark.

- After that, the team create an object named Summarizer Example to run a program in an easier way.

- Then, the team define a function where they start a spark session and load the Summarizer Example that its stored in the Spark installation folder.

- To finalize, they create a data value as an example of information, creating a data frame using that value in two columns, the mean and variance (first values with Summary) and the mean and variance (second values without Summary).

```scala
import org.apache.spark.ml.linalg.{Vector, Vectors}
import org.apache.spark.ml.stat.Summarizer
import org.apache.spark.sql.SparkSession

object SummarizerExample {
  def main(args: Array[String]): Unit = {
    val spark = SparkSession
      .builder
      .appName("SummarizerExample")
      .getOrCreate()

    import spark.implicits._
    import Summarizer._

    val data = Seq(
      (Vectors.dense(2.0, 3.0, 5.0), 1.0),
      (Vectors.dense(4.0, 6.0, 7.0), 2.0)
    )

    val df = data.toDF("features", "weight")

    val (meanVal, varianceVal) = df.select(metrics("mean", "variance")
      .summary($"features", $"weight").as("summary"))
      .select("summary.mean", "summary.variance")
      .as[(Vector, Vector)].first()

    println(s"with weight: mean = ${meanVal}, variance = ${varianceVal}")

    val (meanVal2, varianceVal2) = df.select(mean($"features"), variance($"features"))
      .as[(Vector, Vector)].first()

    println(s"without weight: mean = ${meanVal2}, sum = ${varianceVal2}")

    spark.stop()
  }
}
```