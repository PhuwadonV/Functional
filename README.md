## Example : Functional
```cpp
int f0() {
	return 5;
}

int f1(int i) {
	return i * i;
}

int f2(int i, int j) {
	return i + j;
}

float f3(int i, int j, float f) {
	return (i + j) * f;
}

double f4(int i, long j, float f, double d) {
	return (i * j) + (f * d);
}

int fac(const Applyable<int, int> *f, int i) {
	if (i == 0) return 1;
	else return i * f->apply(i - 1);
}

int fib(const Applyable<int, int> *f, int i) {
	switch (i) {
	case 0: return 0;
	case 1: return 1;
	default: return f->apply(i - 1) + f->apply(i - 2);
	}
}

class C {
	int k;
public:
	C(int k) : k(k) {}

	virtual int f(int i, int j) {
		return i + j + k;
	}

	virtual int g(int i, int j) const {
		return i + j + k;
	}
};

void test() {
	int v0 = 10;
	float v1 = 5.0f;
	char *v2 = "asd";
	const char *v3 = "ASD";

	auto l0 = [v0]() { return 5 * v0; };
	auto l1 = [v0](int i) { return (i * i) * v0; };
	auto l2 = [v0](int i, int j) { return (i + j) * v0; };
	auto l3 = [v0](int i, int j, float f) { return (f * (i + j)) * v0; };
	auto l4 = [v0](int i, long j, float f, double d) { return ((i * j) + (f * d)) * v0; };

	auto cr0 = curry(f2);
	Applyable<int, int, int> *a0 = &cr0;

	C c1(3);
	C *c2 = new C(3);
	const C c3(3);
	const C *c4 = new C(3);

	// Curry : Value
	println(curry(v0));						// Print : 10
	println(curry(v1));						// Print : 5
	println(curry(v2));						// Print : asd
	rintln(curry(v3));						// Print : ASD
	println();

	// Curry : Function
	println(curry(f0));						// Print : 5
	println(curry(f1)(5));					// Print : 25
	println(curry(f2)(5)(10));				// Print : 15
	println(curry(f3)(5)(10)(0.5f));		// Print : 7.5
	println(curry(f4)(5)(10)(0.5f)(5.0));	// Print : 52.5
	println();

	// Curry : Closure
	println(curry(l0));						// Print : 50
	println(curry(l1)(5));					// Print : 250
	println(curry(l2)(5)(10));				// Print : 150
	println(curry(l3)(5)(10)(0.5f));		// Print : 75
	println(curry(l4)(5)(10)(0.5f)(5.0));	// Print : 525
	println();

	// Curry : Curry
	println(typeid(curry(cr0)).name());		
	println(curry(cr0)(5)(10));				// Print : 15
	println();

	// Curry : Applyable Interface			// Print : 15
	println(curry(a0)(5)(10));
	println();

	// Curry : Medthod
	println(curry(c1, &C::f)(5)(7));		// Print : 15
	println(curry(c2, &C::f)(5)(7));		// Print : 15
	println(curry(c1, &C::g)(5)(7));		// Print : 15
	println(curry(c2, &C::g)(5)(7));		// Print : 15
	println(curry(c3, &C::g)(5)(7));		// Print : 15
	println(curry(c4, &C::g)(5)(7));		// Print : 15
	println();

	// Compose Function
	println(compose(f1, [v0](int i) { return i * 10; })(5));	// Print : 2500
	println();

	// Combinator : Y
	println(Combinator::y(fac)(5));			// Print : 120
	println(Combinator::y(fib)(10));		// Print : 55
	println();

	// Tuple
	auto t = Make::tuple(1, 2.0f, "A");	
	println(t.data);						// Print : 1
	println(t.next.data);					// Print : 2
	println(t.next.next.data);				// Print : A
	t.get<2>() = "B";
	println(t.get<0>());					// Print : 1
	println(t.get<1>());					// Print : 2
	println(t.get<2>());					// Print : B
}
```