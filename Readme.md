class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}   

    def rate_lecture(self, lecture, course, grade):
        if isinstance(lecture, Lecture) and course in lecture.courses_attached and course in self.courses_in_progress:
            if course in lecture.grades:
                lecture.grades[course] += [grade]
            else:
                lecture.grades[course] = [grade]
        else:
            return 'Ошибка'

    def _average_grade(self):
        sum_grades, len_grades = 0, 0
        for key, value in self.grades.items():
            sum_grades += sum(value)
            len_grades += len(value)
        return round(sum_grades / len_grades, 1)
  
    def __str__(self):
        name = f'Имя: {self.name}'
        surname = f'Фамилия: {self.surname}'
        average = f'Средняя оценка за лекции: {self._average_grade()}'
        courses_in_progress = f'Курсы в процессе изучения: {", ".join(i for i in self.courses_in_progress)}'
        finished_courses = f'Завершенные курсы: {", ".join(j for j in self.finished_courses)}'
        return name + '\n' + surname + '\n' + average  + '\n' + courses_in_progress + '\n' + finished_courses
      
    def __gt__(self, other):
        if not isinstance(other, Student):
            print('Not a Student!')
            return
        return self._average_grade() > other._average_grade()


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

    def __str__(self):
        name = f'Имя: {self.name}'
        surname = f'Фамилия: {self.surname}'
        return name + '\n' + surname


class Lecture(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}
      
    def _average_grade(self):
        sum_grades, len_grades = 0, 0
        for key, value in self.grades.items():
            sum_grades += sum(value)
            len_grades += len(value)
        return round(sum_grades / len_grades, 1)
  
    def __str__(self):
        name = f'Имя: {self.name}'
        surname = f'Фамилия: {self.surname}'
        average = f'Средняя оценка за лекции: {self._average_grade()}'
        return name + '\n' + surname + '\n' + average

    def __lt__(self, other):
        if not isinstance(other, Lecture):
            print('Not a Lecture!')
            return
        return self._average_grade() < other._average_grade()


class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        name = f'Имя: {self.name}'
        surname = f'Фамилия: {self.surname}'
        return name + '\n' + surname