## Інструменти

Notes:

- Надає інструменти, відсутні в інших мовах
- Розбіжності в коді через різні налагодження
- Той же prettier, jest ін..
- Таби, а не пробіли
+++

### godoc


<video style="max-width: 90%; width: 90%;" type="video/mp4" controls
        src="/slides/03-tools/firefox_19-05-22_0842_0DqctXMDby.mp4"/>

+++

### goembed

```golang
//go:embed migrations/sql/*.sql
var fs embed.FS
```

+++

### Go generate

```goland[1-9|11-20]
// Generate to string converter for enum
//go:generate stringer -type=Pill
const (
    Placebo Pill = iota
    Aspirin
    Ibuprofen
    Paracetamol
    Acetaminophen = Paracetamol
)

const _Pill_name = "PlaceboAspirinIbuprofenParacetamol"

var _Pill_index = [...]uint8{0, 7, 14, 23, 34}

func (i Pill) String() string {
    if i < 0 || i+1 >= Pill(len(_Pill_index)) {
        return fmt.Sprintf("Pill(%d)", i)
    }
    return _Pill_name[_Pill_index[i]:_Pill_index[i+1]]
}
```

Notes:

- альтернатива Make

+++

### Go test

По суті бере твій код і генерує програму що буде виконувати тести

```golang[1-17|17-43]
// Table driven testing
var addTests = []addTest{
    addTest{2, 3, 5},
    addTest{4, 8, 12},
    addTest{6, 9, 15},
    addTest{3, 10, 13}, 
}

func TestAdd(t *testing.T){
    for _, test := range addTests{
        if output := Add(test.arg1, test.arg2); output != test.expected {
            t.Errorf("Output %q not equal to expected %q", output, test.expected)
        }
    }
}

// Advanced example with tables, mocks, parallel execution
func TestDeleteGroup_Fails(t *testing.T) {
	s := setupStabs(t)
	cases := []createUpdateDeleteGroupCases{
		{"create by operator", s.operator, "group1", response_errors.ERROR_ID_UNAUTHORIZED},
		{"create by creator", s.creator, "group1", response_errors.ERROR_ID_UNAUTHORIZED},
		{"create by moderator", s.moderator, "group1", response_errors.ERROR_ID_UNAUTHORIZED},
		{"create by admin with unchanged password", s.adminWithUnchangedPassword, "group1", response_errors.ERROR_ID_UNAUTHORIZED},
	}
	for _, testCase := range cases {
		testCase := testCase
		t.Run(testCase.name, func(tt *testing.T) {
			tt.Parallel()
			_, groupRepository, _, _, _, _, service := setupGroupService(tt)

			groupRepository.EXPECT().Get(gomock.Any(), gomock.Any()).AnyTimes().Return(s.group1, nil)

			displayableError := service.Delete(
				context.Background(),
				testCase.actor,
				s.group1.Id(),
			)

			assert.Equal(tt, testCase.error, displayableError.MessageId())
		})
	}
}
```

+++

<div class="aside">
        <img src="/slides/03-tools/go_bench.jpg">
        <img src="/slides/03-tools/test_coverage.jpg">
</div>

+++

### Інше

- Go vet - щось по типу дефолтного лінтера, рекомендується використовувати golangci-lin, що є агрегатором лінтерів, а також зручно працює з CI платформами
- Go fmt - форматує код - opinionated, немає конфігурації
- Go build flags - можемо вибірково включати файли в білд, наприклад в залежності від версії (free, pro...)
- go race - запускає додаток та відслідковує гонки (race), коли декілька горутин можуть одночасно доступаються до пам'яті
