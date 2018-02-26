
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

