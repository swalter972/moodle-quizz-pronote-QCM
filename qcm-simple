from lxml import etree as ET

def parse_qcm_file(file_path):
    with open('qcm.txt', 'r', encoding='utf-8') as f: #le nom du fihcier est qcm.txt au même endroit que le script
        content = f.read()

    sections = content.split('========') #entre chaque questions
    header = sections[0].strip().split('\n')
    quiz_title = header[0] #le titre du QCM
    quiz_level = header[1] # Le niveau
    quiz_subject = header[2] # l'enseignement

    questions = []
    for section in sections[1:]:
        if section.strip():
            question_parts = section.strip().split('********') #on sépare la question des réponses
            question_text = question_parts[0].strip()
            options = [opt.strip() for opt in question_parts[1].split('|')] :# entre chaque réponse
            questions.append((question_text, options))

    return quiz_title, quiz_level, quiz_subject, questions

def create_xml_quiz(quiz_title, quiz_level, quiz_subject, questions):
    quiz = ET.Element('quiz')

    category_question = ET.SubElement(quiz, 'question', type='category')
    category = ET.SubElement(category_question, 'category')
    text = ET.SubElement(category, 'text')
    text.text = ET.CDATA(f'<infos><name>{quiz_title}</name><answernumbering>123</answernumbering><niveau>{quiz_level}</niveau><matiere>{quiz_subject}</matiere></infos>')

    for question_text, options in questions:
        multichoice_question = ET.SubElement(quiz, 'question', type='multichoice')
        question_text_elem = ET.SubElement(multichoice_question, 'questiontext', format='html')
        text = ET.SubElement(question_text_elem, 'text')
        text.text = ET.CDATA(f'<p>{question_text}</p>')

        for option in options:
            fraction = '100' if '(&&&)' in option else '0'
            option = option.replace('(&&&)', '')
            answer = ET.SubElement(multichoice_question, 'answer', fraction=fraction)
            text = ET.SubElement(answer, 'text')
            text.text = ET.CDATA(option)

    return quiz

def save_xml_file(quiz, file_path):
    xml_string = ET.tostring(quiz, encoding='utf-8', pretty_print=True).decode('utf-8')
    with open(file_path, 'w', encoding='utf-8') as f:
        f.write('<?xml version="1.0" encoding="UTF-8"?>\n')
        f.write(xml_string)

# Utilisation du script
qcm_file = 'qcm.txt'
output_file = 'output.xml'

quiz_title, quiz_level, quiz_subject, questions = parse_qcm_file(qcm_file)
quiz = create_xml_quiz(quiz_title, quiz_level, quiz_subject, questions)
save_xml_file(quiz, output_file)
