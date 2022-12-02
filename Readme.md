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


    def average_grade_students(students, course):
        sum_average, len_average = 0, 0
        for student in students:
            for key, value in student.grades.items():
                if key == course:
                    sum_average += sum(value)
                    len_average += len(value)
        return f'Средняя оценка студентов по курсу {course}: {round(sum_average / len_average, 1)}'


    def average_grade_lectures(lectures, course):
        sum_average, len_average = 0, 0
        for lecture in lectures:
            for key, value in lecture.grades.items():
                if key == course:
                    sum_average += sum(value)
                    len_average += len(value)
        return f'Средняя оценка лекторов по курсу {course}: {round(sum_average / len_average, 1)}'


    best_student = Student('Ruoy', 'Eman', 'your_gender')
    best_student.finished_courses += ['Введение в программирование']
    best_student.courses_in_progress += ['Python', 'Git']
    best_student.grades['Git'] = [10, 10, 10, 10, 10]
    best_student.grades['Python'] = [10, 9, 10, 8, 10]

    good_student = Student('Max', 'Mad', 'your_gender')
    good_student.finished_courses += ['Git']
    good_student.courses_in_progress += ['Python']
    good_student.grades['Git'] = [7, 6, 9, 2, 5]
    good_student.grades['Python'] = [6, 8, 9, 9, 10]

    cool_reviewer = Reviewer('Some', 'Buddy')
    cool_reviewer.courses_attached += ['Python', 'Введение в программирование']

    nice_reviewer = Reviewer('Billy', 'Herrington')
    nice_reviewer.courses_attached += ['Git', 'Python']

    cool_reviewer.rate_hw(best_student, 'Python', 10)
    nice_reviewer.rate_hw(best_student, 'Git', 10)
    cool_reviewer.rate_hw(good_student, 'Python', 8)
    nice_reviewer.rate_hw(good_student, 'Python', 9)

    best_lecture = Lecture('Lionel', 'Messi')
    best_lecture.courses_attached += ['Python', 'Введение в программирование']
    best_lecture.grades['Python'] = [10, 10, 10, 10, 10]

    good_lecture = Lecture('James', 'Bond')
    good_lecture.courses_attached += ['Python', 'Git']
    good_lecture.grades['Python'] = [10, 8, 10, 9, 10, 10, 10]
    good_lecture.grades['Git'] = [0, 0, 7]

    best_student.rate_lecture(best_lecture, 'Python', 10)
    good_student.rate_lecture(best_lecture, 'Python', 10)
    best_student.rate_lecture(good_lecture, 'Git', 7)
    good_student.rate_lecture(good_lecture, 'Python', 9)

    print(cool_reviewer)
    print()
    print(nice_reviewer)
    print()
    print(best_lecture)
    print()
    print(good_lecture)
    print()
    print(best_student)
    print()
    print(good_student)
    print()
    print(best_lecture < good_lecture)
    print(best_student > good_student)
    print()
    list_students = [best_student, good_student]
    print(average_grade_students(list_students, 'Python'))
    list_lectures = [best_lecture, good_lecture]
    print(average_grade_lectures(list_lectures, 'Python'))