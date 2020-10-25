# Refactoring
Mentioned Code: [Quizer](https://github.com/NutthanichN/Quizer), from my previous final project.

### Too many of the same duty:
Mentioned Class: [models.py](https://github.com/NutthanichN/Quizer/blob/master/quizer_game/models.py)  
* Question() and Choice() have too many of the same duty.
```python
class Question(models.Model):
    quiz = models.ForeignKey(Quiz, on_delete=models.CASCADE, verbose_name='Quiz')
    text = models.CharField(max_length=200)
    number = models.IntegerField(default=0, verbose_name='Number')

    def __str__(self):
        return self.text
```
```python
class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE, verbose_name='Question')
    text = models.CharField(max_length=200)
    value = models.IntegerField(default=0, verbose_name='Value')

    def __str__(self):
        return self.text
```
* So, We have to extract superclass and pull up field for factoring.
#### Extract Superclass:
  - We have to create a new superclass.
  ```python
  class Duty(models.Model):
      class Meta:
          abstract = True
  ```
#### Pull Up Field:
  - We have to create a same duty in the superclass and remove them from Question() and Choice().
  ```python
  class Duty(models.Model):
      text = models.CharField(max_length=200)
      number = models.IntegerField(default=0, verbose_name='Number')
        
      class Meta:
          abstract = True
  ```
  ```python
  class Question(Duty):
    quiz = models.ForeignKey(Quiz, on_delete=models.CASCADE, verbose_name='Quiz')

    def __str__(self):
        return self.text
  ```
  ```python
  class Choice(Duty):
    question = models.ForeignKey(Question, on_delete=models.CASCADE, verbose_name='Question')
    value = models.IntegerField(default=0, verbose_name='Value')

    def __str__(self):
        return self.text
  ```
