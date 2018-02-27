
Import('env')

from glob import glob

pdf_to_txt = Builder(suffix='.txt', src_suffix='.pdf', action=Action('pdftk $SOURCE dump_data_fields output $TARGET'))
env['BUILDERS']['PdfToTxt'] = pdf_to_txt

def txt_g():
    for pdf in glob('*.pdf'):
        yield env.PdfToTxt(pdf)

txt = list(txt_g())

import csv
import os

from pprint import pprint

def preferences(target, source, env):
    def to_row(txt):
        def g():
            x, y = os.path.splitext(str(txt))
            yield 'pdf', x + '.pdf'
            with open(str(txt), 'r') as r:
                Y = r.read()
            Y = Y.split('---\n')
            def split(x):
                return [y.strip() for y in x.split(': ')]
            for y in Y:
                if len(y) > 0:
                    X = [x for x in y.split('\n') if len(x) > 0]
                    FieldType, T = split(X[0])
                    FieldName, N = split(X[1])
                    FieldFlags, F = split(X[2])
                    FieldValue, V = split(X[3])
                    assert FieldType == 'FieldType'
                    assert FieldName == 'FieldName'
                    assert FieldFlags == 'FieldFlags'
                    assert FieldValue == 'FieldValue'
                    yield N, V
        return dict(g())
        
    def g():
        for x in source:
            yield to_row(x)

    rows = list(g())

    def g():
        for row in rows:
            for k in row:
                yield k
    fieldnames = sorted(set(g()))

    with open(str(target[0]), 'w') as w:
        writer = csv.DictWriter(w, fieldnames)
        writer.writeheader()
        writer.writerows(rows)

env.Command(target='preferences.csv', source=txt, action=preferences)

import json

config_json = 'config.json'

with open(config_json, 'r') as r:
    CONFIG = json.load(r)

NONE = str(None)

CONFIG['config_json'] = config_json
CONFIG['comma_join_courses'] = ', '.join([NONE] + sorted(CONFIG['courses']))
CONFIG['comma_join_sections'] = ', '.join(str(x) for x in range(CONFIG['max_sections'] + 1))
CONFIG['default_course'] = NONE

def _config_sty_action(target, source, env):
    X = \
r'''% Automatically generated from {config_json}, so edit it instead.
\newcommand{{\MaxPreparations}}{{{max_preparations}}}
\newcommand{{\MaxSections}}{{{max_sections}}}
\newcommand{{\EmailHead}}{{\href{{mailto:{email_head}}}{{\color{{blue}}{email_head}}}}}
\newcommand{{\EmailHeadAssistant}}{{\href{{mailto:{email_head_assistant}}}{{\color{{blue}}{email_head_assistant}}}}}
\newcommand{{\Semester}}{{{semester}}}
'''
    with open(str(target[0]), 'w') as w:
        w.write(X.format(**CONFIG))

config_sty_action = Action(_config_sty_action, '$TARGET: $SOURCE')

env.Command(target=os.path.join('tex', 'config.sty'), source='config.json', action=config_sty_action)

def _teaching_preferences_table_tex_action(target, source, env):
    
    X0 = \
r'''% Automatically generated from {config_json}, so edit it instead.
\newcommand{{\DefaultCourse}}{{{default_course}}}
\begin{{tabular}}{{cccc}}
    Course & \# Sections & Coordinate & Back-to-Back \\
'''

    XR = \
r'''%
    \mbox{{\ChoiceMenu[bordercolor=black, name={course}, mappingname={course}, height=8mm, print, combo, value=\DefaultCourse{{}}, default=\DefaultCourse{{}}]{{}}{{{comma_join_courses}}}}}
    &
    \mbox{{\ChoiceMenu[bordercolor=black, name={course}sections, mappingname={course}sections, height=8mm, print, combo, value=0, default=0]{{}}{{{comma_join_sections}}}}}
    &
    \mbox{{\CheckBox[bordercolor=black, name={course}coordinate, mappingname={course}coordinate, width=8mm, height=8mm]{{}}}}
    &
    \mbox{{\CheckBox[bordercolor=black, name={course}backtoback, mappingname={course}backtoback, width=8mm, height=8mm]{{}}}}
    \\
'''

    X1 = r'''\end{tabular}
'''

    def g():
        yield X0.format(**CONFIG)
        for i in range(CONFIG['max_course_preferences']):
            course = 'course{}'.format(i)
            yield XR.format(course=course, **CONFIG)
        yield X1

    with open(str(target[0]), 'w') as w:
        w.writelines(g())        

teaching_preferences_table_tex_action = Action(_teaching_preferences_table_tex_action, '$TARGET: $SOURCE')

env.Command(target=os.path.join('tex', 'teaching-preferences-table.tex'), source='config.json', action=teaching_preferences_table_tex_action)

from itertools import islice


def _courses_table_tex_action(target, source, env):
    
    C = 4
    courses = sorted(CONFIG['courses'])
    R, r = divmod(len(courses), C)
    if r > 0:
        R += 1

    tabdef = '|' + '|'.join(['rl'] * C) + '|'
    X0 = \
r'''% Automatically generated from {config_json}, so edit it instead.
\begin{{tabular}}{{{tabdef}}}
    \hline
'''

    def g():
        def fill(x):
            while len(x) < R:
                x.append('')
            return x
        def maybe(x):
            if x:
                x = x + ':'
            return x
        it = iter(courses)
        while True:
            row0 = fill(list(islice(it, R)))
            if any(row0):
                row1 = [CONFIG['courses'].get(x, {'days': ''})['days'] for x in row0]
                row0 = [maybe(x) for x in row0]
                yield row0
                yield row1
            else:
                break

    rows = list(g())
    rows = zip(*rows)

    assert len(rows) == R

    XR = '    ' + ' & '.join(['{} & {}'] * C) + r' \\' + '\n'

    X1 = \
r'''%
    \hline
\end{tabular}
'''

    def g():
        yield X0.format(tabdef=tabdef, **CONFIG)
        for row in rows:
            yield XR.format(*row)
        yield X1

    with open(str(target[0]), 'w') as w:
        w.writelines(g())        

courses_table_tex_action = Action(_courses_table_tex_action, '$TARGET: $SOURCE')

env.Command(target=os.path.join('tex', 'courses-table.tex'), source='config.json', action=courses_table_tex_action)

env.PDF(source=os.path.join('tex', 'teaching-preferences.tex'))

