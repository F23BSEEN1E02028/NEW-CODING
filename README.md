class Teacher:
    def __init__(self, teacher_id, name):
        self.teacher_id = teacher_id
        self.name = name
        self.courses = []

    def assign_course(self, course):
        self.courses.append(course)
        course.assign_teacher(self)

    def __str__(self):
        return f"Teacher ID: {self.teacher_id}, Name: {self.name}, Courses: {[course.course_name for course in self.courses]}"


class Course:
    def __init__(self, course_id, course_name):
        self.course_id = course_id
        self.course_name = course_name
        self.teacher = None
        self.students = {}

    def assign_teacher(self, teacher):
        self.teacher = teacher

    def add_student(self, student):
        self.students[student.student_id] = student

    def record_grade(self, student_id, grade):
        if student_id in self.students:
            self.students[student_id].grades[self.course_id] = grade
        else:
            print("Student not found in this course")

    def __str__(self):
        return f"Course ID: {self.course_id}, Name: {self.course_name}, Teacher: {self.teacher.name if self.teacher else 'None'}, Students: {[student.name for student in self.students.values()]}"


class Student:
    def __init__(self, student_id, name):
        self.student_id = student_id
        self.name = name
        self.grades = {}

    def __str__(self):
        return f"Student ID: {self.student_id}, Name: {self.name}, Grades: {self.grades}"


class GradeBook:
    def __init__(self):
        self.teachers = {}
        self.courses = {}
        self.students = {}

    def add_teacher(self, teacher_id, name):
        if teacher_id not in self.teachers:
            teacher = Teacher(teacher_id, name)
            self.teachers[teacher_id] = teacher
            return teacher
        else:
            print("Teacher ID already exists")
            return None

    def add_course(self, course_id, course_name):
        if course_id not in self.courses:
            course = Course(course_id, course_name)
            self.courses[course_id] = course
            return course
        else:
            print("Course ID already exists")
            return None

    def add_student(self, student_id, name):
        if student_id not in self.students:
            student = Student(student_id, name)
            self.students[student_id] = student
            return student
        else:
            print("Student ID already exists")
            return None

    def assign_teacher_to_course(self, teacher_id, course_id):
        if teacher_id in self.teachers and course_id in self.courses:
            teacher = self.teachers[teacher_id]
            course = self.courses[course_id]
            teacher.assign_course(course)
        else:
            print("Invalid teacher ID or course ID")

    def enroll_student_in_course(self, student_id, course_id):
        if student_id in self.students and course_id in self.courses:
            student = self.students[student_id]
            course = self.courses[course_id]
            course.add_student(student)
        else:
            print("Invalid student ID or course ID")

    def record_grade(self, student_id, course_id, grade):
        if course_id in self.courses:
            course = self.courses[course_id]
            course.record_grade(student_id, grade)
        else:
            print("Invalid course ID")

    def __str__(self):
        return f"Teachers: {list(self.teachers.values())}\nCourses: {list(self.courses.values())}\nStudents: {list(self.students.values())}"


# Example usage
gradebook = GradeBook()

# Adding teachers
gradebook.add_teacher(1, "Alice Johnson")
gradebook.add_teacher(2, "Bob Smith")

# Adding courses
gradebook.add_course(101, "Math")
gradebook.add_course(102, "Science")

# Adding students
gradebook.add_student(1001, "John Doe")
gradebook.add_student(1002, "Jane Roe")

# Assigning teachers to courses
gradebook.assign_teacher_to_course(1, 101)
gradebook.assign_teacher_to_course(2, 102)

# Enrolling students in courses
gradebook.enroll_student_in_course(1001, 101)
gradebook.enroll_student_in_course(1002, 102)

# Recording grades
gradebook.record_grade(1001, 101, 95)
gradebook.record_grade(1002, 102, 88)

# Printing out the gradebook
print(gradebook)
