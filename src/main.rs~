use std::{env::args, fs};

fn main() {
    let arg_v: Vec<String> = args().collect();
    if arg_v.len() < 3 {
        panic!("missing args")
    }
    let name = &arg_v[1];
    let search = &arg_v[2];
    for file in fs::read_dir(name).expect("invalid path") {
        match file {
            Ok(v) => {
                let path = v.path();
                let path_s = path.to_str().unwrap();
                if path_s.contains("entities") || path_s.contains("area") {
                    // println!("{path_s}");
                    let mut buf = [0; 100000];
                    let d = &fs::read(path_s).unwrap()[8..];
                    let out_buf = fastlz::decompress(d, &mut buf).unwrap();
                    if out_buf
                        .windows(search.len())
                        .position(|window| window == search.as_bytes())
                        .unwrap_or(0)
                        != 0
                    {
                        let cont: Vec<char> = out_buf
                            .iter()
                            .map(|x| match *x < 127 && 32 <= *x {
                                true => char::from_u32(*x as u32).unwrap(),
                                false => ' ',
                            })
                            .collect();
                        let v: &[char] = &cont;
						let mut c = "".to_owned();
						for ch in v {
							c += &ch.to_string();
						}
                        println!("found {search} at {path_s}:\n{c:?}");
                    }
                }
            }
            Err(_) => println!("something went wrong with file: {file:?}"),
        }
    }
}
